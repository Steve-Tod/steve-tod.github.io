---
title: "SyncDeformNet: Learning Continuous Shape Covariability Across Parametric Models"
collection: projects
type: "Research project"
permalink: /projects/2019-project-SyncDeformNet
date_start: 2018-04-01
date_end: 2019-07-01
excerpt: "We propose a system that can learn the synchronized deformation space for a collection of shapes. <br/><img src='/images/project-sync-visual.png' width='500px'>"
---

Given a 3D shape with deformation parameters and a set of exemplars, we propose a neural-network-based framework finding the whole set of possible variations of the shape over the given parameter space.

First we design a network to deform a parametric source shape into a target shape where we adopt the idea of dictionary learning. The network is divided into a dictionary learning branch and coefficient prediction branch. We learn a source-specific dictionary and coefficient conditioned only on geometry of shapes. In this way the columns of dictionaries of different shapes are indicating visually similar deformation style.

![img](/images/project-sync-net.png)

<p style="text-align: center;">Network</p>

On the one hand, dictionaries learn uniform deformation directions for different shapes. On the other hand, the column space also represents the real sub-manifold in the deformation space. So we can use it in applications like shape editting.