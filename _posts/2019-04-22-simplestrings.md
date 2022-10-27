---
layout: post
title: SimpleStrings
description: Purdue ME 2019 Capstone Project
image: assets/images/simple-strings/main.png
imagewidth: 0
---

Mechanical engineering students at Purdue University participate in a semester long capstone project in teams of six. The goal of the project is to propose, design, and produce a prototype of a product to solve a problem of the team's choice. My team decided to combine our experience with mechanical, electrical, and software engineering with our passion for music to create **SimpleStrings**, an assistive guitar playing device.


{% include youtube.html video_id="94spG3D15ZU" width="75%" %}

<br>



## Problem Definition
SimpleStrings was designed as a tool for music therapy. [**Music Therapy**](https://www.musictherapy.org/about/musictherapy/) is the practice of using music in a therapeutic relationship to address physical, emotional, cognitive, and social needs of individuals. It can be used to help individuals in many situations, including **brain injuries**, **dementia**, **dyslexia**, **autism**, **mental illnesses**, and more.

**Guitar can be a challenging instrument to use in music therapy situations**. It involves complex hand configurations and memorization to produce chords. In situations where therapy is the end goal (not learning to play the instrument itself), this learning curve can cause frustration on the part of the player. During our initial research phase, my teams spoke to 9 music therapy professionals and 2 special educators. We learned from them that some of their patients love the guitar, and a method of facilitating access to the instrument would improve their music therapy experience and outcomes.

With that in mind, we set out to design a device that would **make it easier for individuals with varying physical and/or mental abilities to play the guitar**.

## Team Organization
In order to complete the entire design process from conceptual design to prototype fabrication in just 5 months, our team assigned roles to allow us to focus our efforts more efficiently as a team. The team consisted of:

| **Kristen Gallett** - Project Manager | **Jared Borg** - Chief Engineer |
| **Emily Jackson** - Procurement, Safety Lead | **Jonathon Lemke** - CAD Lead, Business Manager |
| **Lindsey May** - Software Lead, Customer Eyes | **Nick Morales** - Electronics Lead |

## Design
The SimpleStrings device is designed to attach directly to the neck of a guitar (this prototype was built for a Fender Squier Stratocaster). Its working area covers **4 frets at a time**, but the device can be repositioned in between songs to **5 different positions** along the guitar neck. This means that, if used with a standard guitar capo, SimpleStrings can be used to play music in any desired key.

{:refdef: style="text-align: center;"}
![SimpleStrings attached to the guitar neck with a capo](/assets/images/simple-strings/simplestrings-with-capo.png){: width="40%" style="padding-right: 5%;" }
![The fret range of the SimpleStrings device](/assets/images/simple-strings/fret-range.png){: width="40%" style="padding-bottom: 7.5%;"}
{: refdef}

When one of the colored buttons along the bottom of the device is pressed, 24 small gear motors inside the device are actuated to press down on the guitar strings in specific configurations, forming chords. The buttons do not need to be held down and there is no need to memorize/master complex hand positions, greatly facilitating the mechanical aspects of playing the guitar.

{:refdef: style="text-align: center;"}
![The underside of SimpleStrings](/assets/images/simple-strings/undercarriage.png){: width="40%" style="padding-right: 5%;" }
![The fret range of the SimpleStrings device](/assets/images/simple-strings/micro-gearmotor.png){: width="40%"}
{: refdef}

5 colored buttons allow for 5 main chords to be stored at a time, and 2 "modifier" buttons can modify each of those chords for a little extra flair (ex: Am -> Am7 or D -> Dsus4). This allows the device to store up to **15 programmable chords at a time**. These chords are completely customizable through our custom chord programming application, which allows users to **select from 142 default chords or define any completely custom chord** in the 4 fret range. The currently programmed chords appear on the OLED screens on the top of the device.

{:refdef: style="text-align: center;"}
![Custom interface used for selecting the device's stored chords](/assets/images/simple-strings/chord-programming.png){: width="40%" style="padding-right: 5%;" }
![OLED screens display the currently programmed chords and their modifiers](/assets/images/simple-strings/oled-screens.png){: width="25%" }
{: refdef}

### Characteristics/Functionality

{:refdef: style="text-align: center;"}
![Playing SimpleStrings](/assets/images/simple-strings/playing-simplestrings-1.png){: width="60%" }
{: refdef}

| **Battery Life** | ~3.5 hours of play time per battery |
| **Button Press Force** | 11% of normal string depression force |
| **Device Attachment Time**| ~47 seconds |
| **Chord Change Time**| 0.42 seconds |
| **Dimensions**| 18.75 cm x 16.25 cm x 11.5 cm |
| **Weight**| 1.185 kg (user support is not necessary to keep the guitar from tipping with SimpleStrings attached) |

{:refdef: style="text-align: center;"}
![Physical characteristics of SimpleStrings](/assets/images/simple-strings/dimensions-and-weight.png){: width="75%" }
{: refdef}

### Electrical Subsystem
My primary responsibility. Explain TODO


## Future Improvements
TODO
Our own improvements as well as feedback from music therapists.
- Buttons hard to see
- Indication of which chords is played
- Height is very cumbersome (insert pictures)






And finally, here's an extended cut of the project video!
{% include youtube.html video_id="d3h6AYNHfU4" width="75%"%}