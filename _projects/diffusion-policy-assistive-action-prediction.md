---
layout: project
title:  Diffusion Policy Action Prediction
description: C++, PyTorch, Machine Learning, ROS 2
image: assets/images/omnid-mocobots/main.gif # TODO
imagewidth: 0
order: 986
---

For my final project in Northwestern University's MSR program, I worked with the unique [**omnidirectional mobile cobots**](https://www.mccormick.northwestern.edu/news/articles/2022/08/mobile-cobots-offer-glimpse-of-future-of-human-robot-interaction/), dubbed "omnid mocobots" or "omnids" for short. These collaborative robots were developed at Northwestern University by [**Elwin et al.**](https://arxiv.org/abs/2206.14293).

A portion of the project involved [upgrading the omnids' onboard systems](/projects/omnid-mocobots), but the main goal of the project was to research novel ways to apply **assistive action prediction using generative machine learning models**. To do this, I applied [**diffusion policy**](https://diffusion-policy.cs.columbia.edu/), a method of generating robot actions using [diffusion models](TODO).

TODO - video

TODO - table of contents

****

## Diffusion Policy

### What is a diffusion model?
[Diffusion models](https://en.wikipedia.org/wiki/Diffusion_model) were introduced in 2015 as a method of learning a model that can sample from highly complex probability distributions. The models used in this project are based on the [denoising diffusion probabilistic model (DDPM)](https://en.wikipedia.org/wiki/Diffusion_model), introduced in 2020.

A classic use of this type of model is image generation, where by training the model on a set of images, the model could then generate new images that approximate the input set.

{:refdef: style="text-align: center;"}
![DDPM Image Generation](/assets/images/diffusion-policy-assistive-action-prediction/ddpm-image-generation.png){: width="70%"}
{: refdef}
{:refdef: style="text-align: center;"}
_A diagram of how DDPM models generate new images. The example images are from a simple model I trained on the Caltech 101 Motorbikes dataset._
{: refdef}

{% details **<u>Expand</u>** for more details on image generation with diffusion models. %}

To do this, the process takes images from the input dataset and gradually introduces more and more Gaussian noise into them over _k_ timesteps. Once enough of this noise is added, all of the original image data is lost, and all that remains is Gaussian noise. A neural network can subsequently be trained to take the diffusion timestep _k_ and the noisy image at that timestep and "predict the noise" that was added to that image from the previous timestep.

Once the network has been trained, the process can then be reversed. An image consisting purely of random Gaussian noise can be generated. Then, over _k_ timesteps, the model "predicts and removes the noise" iteratively, until a newly generated image is created.

Importantly, the model can also be conditioned on other inputs, such as a text prompt. At a high level, this is how many popular image generation models (such as Stable Diffusion and DALL-E 2) generate images from a text prompt.

{% enddetails %}

<br>

### What are diffusion models actually doing?
"Removing noise" from data (i.e. recovering information from nothing) is not possible - hence the popular description of the reverse diffusion process as "removing noise" to generate new images is not accurate. Instead the diffusion model is learning the **transfer function** between an arbitrary distribution and a Gaussian distribution. This is very useful, as it allows one to take a sample from a Gaussian distribution (which is simple to sample from) and transform it into a sample from an arbitrarily complex distribution of one's choosing.

{:refdef: style="text-align: center;"}
![DDPM Distribution Sampling](/assets/images/diffusion-policy-assistive-action-prediction/ddpm-distribution-sampling.png){: width="70%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Diffusion models learn an incremental transfer function that allows sampling from an arbitrary distribution that represents the data input into the model._
{: refdef}

### How can diffusion models generate robot actions?

****

## Machine Learning Pipeline

### Data Collection

### Training

### Evaluation

****

## Results

****

## Future Work