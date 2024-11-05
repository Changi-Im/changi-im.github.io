---
layout: page
title: Image Quality Improvement Through Image Synthesis on Edge Device
description: 
img: assets/img/edge_device_IQ/method.png
importance: 1
category: work
related_publications: false
---

Video synthesis using artificial intelligence neural networks provides superior image quality compared to traditional synthesis methods, but it requires a significant amount of computation. While Graphics Processing Units (GPUs) are primarily used for training and inference of artificial intelligence neural networks, they are often unsuitable for real-time applications due to performance limitations or high costs. Therefore, we have designed a lightweight Convolutional Neural Network (CNN) using the MobileNet structure and bit optimization techniques. Additionally, we have developed a code that can be executed on a Field-Programmable Gate Array (FPGA) board.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/edge_device_IQ/method.png" title="method" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The process of proposed algorithm.
</div>

The code used in the project is available in the github repository (<a href="https://github.com/Changi-Im/Development-of-Image-Synthesis-Edge-Device-for-Image-Quality-Improvement/tree/master">Development of Image Synthesis Edge Device for Image Quality Improvement</a>).