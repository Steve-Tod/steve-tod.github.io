---
title: Spatial Transformer Networks
date: 2018-10-06 16:50:56
tags:
- 科研
- 论文
- CV
---

## 目录
- [思想介绍](#intro)
- [原理推导](#theory)
- [简单实现](#implement)
- [总结](#conclusion)

<!--more-->

### <a name="intro"/>思想介绍

[Spatial Transformer Networks](https://arxiv.org/abs/1506.02025) 引入了一种新的可学习的模块 Spatial Transformer 。这种模块能够对输入的图像（或者 feature map）进行针对性变换，变换的参数根据输入图像计算出来的，而计算过程的参数是通过学习得到的。这种变换赋予网络空间不变性，也就是对于旋转，移动，拉伸或扭曲的图像，一样具有一定的识别能力，这就是通过 Spatial Transformer 模块将原图变为“正常”的样子来实现的。

原论文中，Spatial Transformer 的构造如下图：

![Spatial Transformer](/assets/img/spatial_transformer.png)

首先通过 Localisation net 计算出变换的参数，然后通过 Grid generator 生成变换的栅格，再根据栅格采样获得变换后的图像。中间的 Localisation net 参数是可学习的，需要让导数传播到栅格数据从而传播到网络中，导数传播的推导见下节。

### <a name="theory"/>原理推导

以仿射变换变换为例

$$
\left\( \begin{matrix} 
x_i^s \\\ 
y_i^s 
\end{matrix} \right \)
= T(G_i) =
\begin{bmatrix}
\theta_{11} & \theta_{12} & \theta_{13} \\\
\theta_{21} & \theta_{22} & \theta_{23}
\end{bmatrix}
\left \( \begin{matrix}
x_i^t \\\ y_i^t \\\ 1
\end{matrix} \right \)
$$

这里 $(x_i^t, y_i^t)$ 是输出图上第 i 个像素点的位置，对应的，$(x_i^s, y_i^s)$ 就是原图中对应像素点的位置，根据仿射变换的结果，可以得到输出的每个点对应到原图中的哪个位置，这就是栅格数据。

获得栅格数据之后，利用双线性插值即可获得一个位置处的像素值，公式如下：

$$V_i^c = \sum_n^H\sum_m^W U_{mn}^c \max (0, 1- |x_i^s-m|) \max (0, 1 - |y_i^s-n|)$$

$V_i^c$ 是输出的第 i 个像素点的数值，H 和 W 分别是原图的高和宽，上式就是遍历原图像素点，找到与目标点相邻的像素再插值。

根据上面的公式，就可以计算导数了：

$$\frac{\partial V_i^c}{\partial U_{mn}^c} = \sum_n^H\sum_m^W {mn}^c \max (0, 1- |x_i^s-m|) \max (0, 1 - |y_i^s-n|)$$

上式计算的是对于原图的导数，如果输入的是一张 feature map 的话，导数就可以继续传播下去了。

$$\frac{\partial V_i^c}{\partial x_i^s} =  \sum_n^H\sum_m^W U_{mn}^c \max (0, 1 - |y_i^s-n|) \times 
\left\( \begin{matrix}
0  & |m-x_i^s|\ge 1 \\\ 1 &m \ge x_i^s \\\ -1  & m < x_i^s
\end{matrix} \right.
$$

这样就可以计算对于 $(x_i^s, y_i^s)$ 的导数，从而计算对于$\theta$矩阵的导数，进而计算对于 Localisation net 参数的导数了。

### <a name="implement"/> 简单实现

我搬运 GitHub 里的一个 pytorch 实现 STN 的[仓库](https://github.com/WarBean/tps_stn_pytorch)，简化后实现了一个只做旋转变换的 STN 。

输入数据就是在一定角度范围内旋转过的 MNIST 手写数字图片。

模型代码如下：

下面是一个基本的 CNN 模块，后续的 Localisation net 和 分类网络都是用的这个模块。

```python
class CNN(nn.Module):
    def __init__(self, num_output):
        super(CNN, self).__init__()
        self.conv1 = nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = nn.Conv2d(10, 20, kernel_size=5)
        self.conv2_drop = nn.Dropout2d()
        self.fc1 = nn.Linear(320, 50)
        self.fc2 = nn.Linear(50, num_output)

    def forward(self, x):
        x = F.relu(F.max_pool2d(self.conv1(x), 2))
        x = F.relu(F.max_pool2d(self.conv2_drop(self.conv2(x)), 2))
        x = x.view(-1, 320)
        x = F.relu(self.fc1(x))
        x = F.dropout(x, training=self.training)
        x = self.fc2(x)
        return x
```

下面是分类模块。

```python
class ClsNet(nn.Module):

    def __init__(self):
        super(ClsNet, self).__init__()
        self.cnn = CNN(10)

    def forward(self, x):
        return F.log_softmax(self.cnn(x), dim=1)

```

下面是 Localisation net ，可以看出，仅仅是将 CNN 模块的输出改为 1 维的。

```python
# get rotate theta
class LocNet(nn.Module):

    def __init__(self):
        super(LocNet, self).__init__()
        self.cnn = CNN(1)

        # zero init
        bias = torch.zeros(1)
        self.cnn.fc2.bias.data.copy_(bias)
        self.cnn.fc2.weight.data.zero_()

    def forward(self, x):
        batch_size = x.size()[0]
        theta = self.cnn(x)
        return theta.view(batch_size)
```

下面是旋转模块，根据角度构建仿射矩阵，然后用 pytorch 自带的 affine\_grid 生成栅格。

```python
# rotate
class RotateGridGen(nn.Module):

    def __init__(self):
        super(RotateGridGen, self).__init__()

    def forward(self, theta, out_size):
        assert len(theta.size()) == 1
        assert type(out_size) == torch.Size
        batch_size = theta.size()[0]
        affine_mat = theta.new(batch_size, 2, 3)
        affine_mat[:, :, 2] = 0
        affine_mat[:, 0, 0] = torch.cos(theta)
        affine_mat[:, 1, 1] = torch.cos(theta)
        affine_mat[:, 0, 1] = -torch.sin(theta)
        affine_mat[:, 1, 0] = torch.sin(theta)
        grid = F.affine_grid(affine_mat, out_size)
        return grid
```

下面是整体的网络，用 pytorch 自带的 grid\_sample 实现双线性插值。

```python
# Full net
class STNClsNet(nn.Module):

    def __init__(self):
        super(STNClsNet, self).__init__()

        self.loc_net = LocNet()
        self.rotate = RotateGridGen()
        self.cls_net = ClsNet()

    def forward(self, x):
        batch_size = x.size()[0]
        theta = self.loc_net(x)
        grid = self.rotate(theta, x.size())
        transformed_x = F.grid_sample(x, grid)
        logit = self.cls_net(transformed_x)
        return logit
```
Localisation net 在测试集上的部分输出结果如下图，上面是原图，下面是变换结果：

![Result](/assets/img/rotate_stn_res.jpg)

可以看到，在大多数输入中，Spatial Transformer 成功地将输入图片给“拧正”了，也就是说，在端到端的分类任务中，它学会了如何将数据旋转至便于后续网络分类的形式。

完整代码见我的[仓库](https://github.com/Steve-Tod/rotate_stn_pytorch)

###  <a name='conclusion'/> 总结
Spatial Transformer Networks 提出了一种全新的模块，在端到端的学习中，可以弱监督地学会对输入图片（及特征图）进行空间变换，以方便后续网络完成任务。此外，习得的变换网络的参数也可以用于其他任务。
