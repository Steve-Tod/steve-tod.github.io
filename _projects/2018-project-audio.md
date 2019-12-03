---
title: "Audio source separation and sound localization"
collection: projects
type: "Course project"
permalink: /projects/2018-project-audio
date_start: 2018-09-01
date_end: 2019-01-01
excerpt: "We build a system that can separate the mixture of two different audio sources and localize the sources from video ."
---

For input audios, we divide them into segments and calculate STFT results of them. Then we feed the STFT into a modified UNet and output a mask of 8 channels, representing the masks for 8 different instruments respectively.

![img](/images/project-audio-net.png)

<p style="text-align: center;">Modified UNet for audio separation</p>

Then we analyze the audio. By feeding the frames into pre-trained DenseNet161, we can get the scores for 8 possible instruments and calculate Class Activation Map (CAM)[^ref1] for each instruments. Using CAM, we can do a weakly-supervised detection and localize the instruments with highest probability.

By selecting the masks representing the most possible instruments, we can get the separated STFT spectrums by multiplying the masks with the input. Then using iSTFT we can recover the separated sound.

[^ref1]: Zhou, Bolei, et al. "Learning deep features for discriminative localization." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016.
