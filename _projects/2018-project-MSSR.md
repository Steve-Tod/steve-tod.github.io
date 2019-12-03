---
title: "Improving Image Super-Resolution via Multi-Scale Discrimination"
collection: projects
type: "Research project"
permalink: /projects/2018-project-MSSR
date_start: 2018-11-01
date_end: 2019-03-01
excerpt: "We propose a powerful SR method with multi-scale discrimination based on the scale-invariance property of the task of SR. We also propose a powerful metric RMR to evaluate the perceptual performance of SR methods taking advantage of the solutions to binary classification. <br/><img src='/images/project-MSSR.png' width='500px'>"
---

Details matter in image super-resolution. Recently, many methods are proposed to improve the fidelity of super-resolved images by adopting the idea of the generative adversarial network (GAN). However, these methods only focus on global fidelity without explicitly considering the detail quality of local patches. We argue that super-resolution problem is scale-invariant, where any part in an ideally super-resolved image should also have high perceptual quality.

We present a new super-resolution framework based on multi-scale discrimination to boost the performance of super-resolution.  Specifically, we propose to use a sequence of pyramidal discriminators instead of a single one to supervise generative model, so that multiple patches of various scales from the super-resolved image are explicitly constrained to be photo-realistic simultaneously. Empirically, we show our method can better capture details and achieve superior performance on several widely used datasets.

![img](/images/project-FaceSR-framework.png)

<p style="text-align: center;">Framework</p>

In addition, we present a new evaluation metric, relative misclassification rate (RMR), to measure the quantitative performance of SR methods. We evaluate a number of recent SR approaches based on our metric and demonstrate the proposed metric can better measure perceptual quality.
