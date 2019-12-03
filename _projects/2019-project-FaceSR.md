---
title: "Deep Face Super-Resolution with Iterative Collaboration between Attentive Recovery and Landmark Estimation"
collection: projects
type: "Research project"
permalink: /projects/2019-project-FaceSR
date_start: 2018-04-01
date_end: 2019-07-01
excerpt: "We propose a deep iterative collaboration network for face super-resolution. <br/><img src='/images/project-FaceSR-head.png' width='500px'>"
---

Recent works based on deep learning and facial priors have succeeded in super-resolving severely degraded facial images. However, the prior knowledge is not fully exploited in existing methods, since facial priors such as landmark and component maps are always estimated by low-resolution or coarsely super-resolved images, which may be inaccurate and thus affect the recovery performance.

In this paper, we propose a deep face super-resolution (FSR) method with iterative collaboration between two recurrent networks which focus on facial image recovery and landmark estimation respectively. In each recurrent step, the recovery branch utilizes the prior knowledge of landmarks to yield higher-quality images which facilitate more accurate landmark estimation in turn. Therefore, the iterative information interaction between two processes boosts the performance of each other progressively.

![img](/images/project-FaceSR-net.png)

<p style="text-align: center;">Framework</p>

Moreover, a new attentive fusion module is designed to strengthen the guidance of landmark maps, where facial components are generated individually and aggregated  attentively for better restoration. Quantitative and qualitative experimental results show the proposed method significantly outperforms state-of-the-art FSR methods in recovering high-quality face images.

![img](/images/project-FaceSR-fusion.png)

<p style="text-align: center;">Fusion module</p>
