---
title: "Object classification and weakly-supervised object detection"
collection: projects
type: "Course project"
permalink: /projects/2019-project-object
date_start: 2017-11-01
date_end: 2018-10-01
excerpt: "We built a system that can do multi-label object classification and weakly-supervised object detection"
---

### Multi-label object classification

We implement ML_GCN[^ref1] for multi-label object classification.

### Weakly-supervised object detection

We adopted CAM[^ref2] as main method and Hide-and-Seek[^ref3] as additional training strategy to detect the object in a weakly-supervised fashion.

![img](/images/project-object-result.png)

<p style="text-align: center;">Some test results. Heatmaps represent CAM. Red boxes represent detection results while green boxes represent ground truth</p>

[^ref1]: Chen, Zhao-Min, et al. "Multi-Label Image Recognition with Graph Convolutional Networks." Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2019.

[^ref2]: Zhou, Bolei, et al. "Learning deep features for discriminative localization." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016.

[^ref3]: Singh, Krishna Kumar, and Yong Jae Lee. "Hide-and-seek: Forcing a network to be meticulous for weakly-supervised object and action localization." 2017 IEEE International Conference on Computer Vision (ICCV). IEEE, 2017.
