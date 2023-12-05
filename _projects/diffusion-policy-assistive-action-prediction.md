---
layout: project
title:  Diffusion Policy Action Prediction
description: C++, PyTorch, Machine Learning, ROS 2
image: assets/images/omnid-mocobots/main.gif # TODO
imagewidth: 0
order: 986
---

TODO - github

For my final project in Northwestern University's MSR program, I worked with the unique [**omnidirectional mobile cobots**](https://www.mccormick.northwestern.edu/news/articles/2022/08/mobile-cobots-offer-glimpse-of-future-of-human-robot-interaction/), dubbed "omnid mocobots" or "omnids" for short. These collaborative robots were developed at Northwestern University by [**Elwin et al.**](https://arxiv.org/abs/2206.14293).

A portion of the project involved [upgrading the omnids' onboard systems](/projects/omnid-mocobots), but the main goal of the project was to research novel ways to apply **assistive action prediction using generative machine learning models**. To do this, I applied [**diffusion policy**](https://diffusion-policy.cs.columbia.edu/), a method of generating robot actions using [diffusion models](#diffusion-policy).

TODO - video

TODO - results summary

TODO - table of contents

****

## Assistive Action Prediction with the Omnids

### Omnid System

The omnids were designed as an assistive tool to help human operators manipulate potentially large, delicate, flexible, and/or articulated payloads.

Each omnid consists of an omnidirectional mobile base and a series-elastic actuator driven Delta parallel manipulator on top. During the robot's "float" mode, the manipulator is force-controlled to cancel the gravity of a payload and zero the contact force. The mobile base is commanded to center itself under the end-effector. This combination allows a human collaborator to easily guide the payload as it is carried by the omnids.

{:refdef: style="text-align: center;"}
![Omnid System](/assets/images/diffusion-policy-assistive-action-prediction/omnid-system.png){: width="40%"}
{: refdef}

While they are also capable of autonomous action, the omnid team at Northwestern was particularly interested in exploring whether applying generative action prediction through models like diffusion policy could improve omnid performance in collaborative tasks.

Questions asked included:
- Will action prediction in this system help reduce the force the human must apply to manipulate the payload?
- Will action prediction in this system help the human control the payload more precisely?

Several potential system architectures were designed to explore these questions. In each architecture, the generative model is used to produce an action, which is then executed by the system. The model in each architecture takes as input:
- End-effector X/Y/Z positions, velocities, and forces
- Gimbal X/Y/Z axis rotations
- Base twist (optional) from the float controller
- Image data (optional) from three cameras (overhead, horizontal, and onboard the omnid)

### Force Prediction

The idea behind the force prediction is that if the model can predict the force applied by the human on the end-effector, the force controller can preemptively apply that force to assist the human. The predicted force can then be fed into the end-effector's force controller as additional forces to apply to the end-effector. Optionally, the feedback end-effector force values can be subtracted from the prediction, so only the "residual" is fed into the force controller.

{:refdef: style="text-align: center;"}
![Force Prediction Setup](/assets/images/diffusion-policy-assistive-action-prediction/force-prediction.png){: width="50%"}
{: refdef}

### Position Prediction

Similarly, position prediction focuses on predicting the position of the end-effector (from its home position) and preemptively commanding the end-effector to that prediction via a position controller.

{:refdef: style="text-align: center;"}
![Position Prediction Setup](/assets/images/diffusion-policy-assistive-action-prediction/position-prediction.png){: width="50%"}
{: refdef}

### Base Twist Prediction

Finally, base twist prediction could either replace the float controller entirely or augment it with "residual" values. With this architecture, the base would follow a twist that anticipates the human's actions.

{:refdef: style="text-align: center;"}
![Base Twist Prediction Setup](/assets/images/diffusion-policy-assistive-action-prediction/twist-prediction.png){: width="50%"}
{: refdef}

****

## Diffusion Policy

[**Diffusion policy**](https://diffusion-policy.cs.columbia.edu/) was introduced in 2023. This policy applies diffusion models (a type of generative machine learning model) to the generation of robotic action sequences based on a dataset of human demonstrations. The action generation is conditioned on observations from the robot's sensors, allowing the robot to consider its state and the state of the world around it while planning actions.

{% details **<u>Expand</u>** for more details on diffusion models and how diffusion policy works.%}

### What is a diffusion model?
[Diffusion models](https://en.wikipedia.org/wiki/Diffusion_model) were introduced in 2015 as a method of learning a model that can sample from highly complex probability distributions. The models used in this project are based on the [denoising diffusion probabilistic model (DDPM)](https://en.wikipedia.org/wiki/Diffusion_model#Denoising_diffusion_model), introduced in 2020.

A classic use of this type of model is image generation, where by training the model on a set of images, the model can then generate new images that approximate the input set.

{:refdef: style="text-align: center;"}
![DDPM Image Generation](/assets/images/diffusion-policy-assistive-action-prediction/ddpm-image-generation.png){: width="70%"}
{: refdef}
{:refdef: style="text-align: center;"}
_This example is from a [simple model](https://www.youtube.com/watch?v=a4Yfz2FxXiY) I trained on the [Caltech 101](https://data.caltech.edu/records/mzrjq-6wc02) Motorbikes dataset._
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
_This example is from modifying the above image diffusion model and retraining it on an arbitrary multimodal 1D distribution._
{: refdef}

### How can diffusion models generate robot actions?

[**Diffusion policy**](https://diffusion-policy.cs.columbia.edu/) was introduced in 2023. This policy applies diffusion models to the generation of action sequences based on a dataset of human demonstrations. Much like image generation models can condition their generation on text prompts, diffusion policy conditions the action sequence generation on the observations from the robot's sensors - cameras, position/velocity/force feedback, and more.

{:refdef: style="text-align: center;"}
![Diffusion Policy Action Prediction](/assets/images/diffusion-policy-assistive-action-prediction/diffusion-policy-action-prediction.png){: width="70%"}
{: refdef}

After predicting an action sequence across a **prediction horizon (_Tp_)** based on observations from an **observation horizon (_To_)**, diffusion policy then performs **receding horizon control**. This means that only a subset **action horizon (_Ta_)** of actions is performed before the model performs a new prediction based on updated observations. Then the whole process repeats.

{:refdef: style="text-align: center;"}
![Receding Horizon Control with Diffusion Policy](/assets/images/diffusion-policy-assistive-action-prediction/receding-horizon-control.png){: width="70%"}
{: refdef}

The advantage of using a diffusion model to plan robot actions like this is that, as mentioned above, diffusion models can sample from complex arbitrary distributions. Robot action sequence distributions are often complex and multimodal - there are often many ways a robot can accomplish a certain task, and diffusion policy handles those complexities gracefully.

{% enddetails %}

****

## Machine Learning Pipeline

In order to test the above architectures, a pipeline needed to be created with infrastructure for collecting data, training, and testing.

### Data Collection

To collect human demonstration data, a simple task space was set up. This space included several differently-colored tape targets to indicate positions to which the human should guide an omnid.

{:refdef: style="text-align: center;"}
![Task Space](/assets/images/diffusion-policy-assistive-action-prediction/task-space.png){: width="70%"}
{: refdef}
{:refdef: style="text-align: center;"}
_The task space in which all testing was performed._
{: refdef}

50 human demonstrations were collected for each target, by having the human drag an omnid by a small payload dubbed the "leash". AprilTags are fixed to the floor, the robot, and the leash, allowing each to be tracked later for evaluation of any collected data. AprilTags detections are not passed as inputs to any models.

{:refdef: style="text-align: center;"}
![Omnid Homing Setup](/assets/images/diffusion-policy-assistive-action-prediction/omnid-homing-setup.jpg){: width="60%"}
{: refdef}
{:refdef: style="text-align: center;"}
_The "leash" - a small payload constructed of 80/20 with an AprilTag attached to it.<br>This rests on the "rack", a rig used to home the Delta manipulator to a consistent position before every test._
{: refdef}

{:refdef: style="text-align: center;"}
![Dataset Trajectories](/assets/images/diffusion-policy-assistive-action-prediction/dataset-trajectories.png){: width="40%"}
{: refdef}
{:refdef: style="text-align: center;"}
_A plot showing the all trajectories followed by the robot in the training dataset._
{: refdef}

Three cameras were used to collect image data - an overhead camera, a horizontal camera, and a camera onboard the omnid.

{:refdef: style="text-align: center;"}
![Example Training Run](/assets/images/diffusion-policy-assistive-action-prediction/example-training-run.gif){: width="100%"}
{: refdef}
{:refdef: style="text-align: center;"}
_The camera feed from each camera during a run._
{: refdef}

In order to record all this data, an `omnid_data_collection` package was written for ROS 2 Iron. This package included a node that recorded ROS bags based on configured input topics, launch files to start all required nodes, and bash helper scripts to guide users through the data collection process. The launch files relied heavily on the [`launch_remote_ssh`](/projects/omnid-mocobots#launch_remote_ssh) package I wrote to launch all required nodes on multiple machines simultaneously.

This infrastructure was critical in collecting data quickly and allowed a **full training human demonstration run to be recorded every 40 seconds**.

{% details **<u>Expand</u>** for a full list of data collected from runs.%}
This includes data that may only be meaningful on evaluation runs (with a generative model running).
- Overhead camera images/info
- Horizontal camera images/info
- Onboard camera images/info
- TF tree (for AprilTag transforms)
- AprilTag detections
- Joint states of all joints (including position, velocity, and effort)
- Commanded additional forces for EE force controller
- Commanded Delta motor velocity or torque
- Commanded base twist
- Payload's reflected weight
- Estimated odometry
- Target positional achievement status
- Details of generative model used (if any)
{% enddetails %}

<br>

### Training

To train diffusion policy models, I [forked the diffusion policy repository](https://github.com/ngmor/diffusion_policy) and set it up for use with omnid tasks.

Since the [action prediction and receding horizon control](#how-can-diffusion-models-generate-robot-actions) used in diffusion policy relies on discrete timeframes of data, the ROS bag data had to be decimated at a certain rate. For models using image data, I chose this rate to be 15 Hz (one half of the camera refresh rate). For models not using image data, I chose this rate to be 50 Hz (one half of the joint state refresh rate). Data was decimated with a script that averaged collected data over the timeframe period.

For all the models I chose to use a neural network architecture based on the convolutional [U-Net](https://en.wikipedia.org/wiki/U-Net) architecture, since the diffusion policy paper suggested that this may be sufficient for most tasks and easier to tune than the alternative transformer architecture.

Training plots typically looked something like this, with validation loss quickly hitting a minimum and then rising steadily. Validation loss rising like this typically indicates overfitting. In the case of generative model like this one, however, overfitting may not lead to worse performance.

{:refdef: style="text-align: center;"}
![Training Plots](/assets/images/diffusion-policy-assistive-action-prediction/training-plots.png){: width="100%"}
{: refdef}

{% details **<u>Expand</u>** for more detail on the code required to get this model training. %}
Luckily, the diffusion policy repository is generally well set up for quickly adding new tasks - but the cost of this flexibility is that the [codebase structure](https://github.com/ngmor/diffusion_policy#%EF%B8%8F-codebase-tutorial) takes some time to understand. In the end, since I was only creating new tasks, I could get it to work by simply adding new PyTorch dataset loaders, environment runners, and [hydra](https://hydra.cc/) task configurations. Finally, training data had to be converted from from ROS bag data to the [`zarr`](https://zarr.readthedocs.io/en/stable/index.html) format expected as input to the training pipeline. Decimation occurred during this conversion.
{% enddetails %}

<br>

### Evaluation

#### Gauntlet Task

****

## Results

### Architecture Selection

****

## Future Work

- autonomy
- model tuning (hyperparameters, decimation rate)
- other model types ("oversmoothing" from CNNs for fast refresh rates)
- further explore whether more or less training is better
- better evaluation