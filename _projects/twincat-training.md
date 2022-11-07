---
layout: project
title: TwinCAT Training Material
description: Beckhoff How-Tos and Training Labs
image: assets/images/twincat-training/main.png
imagewidth: 90%
order: 999
---

During my time with the automation team at **[DMC, Inc](https://www.dmcinfo.com/)**, I became somewhat of a subject matter expert with **[Beckhoff's TwinCAT 3 platform](https://www.beckhoff.com/en-us/products/automation/twincat/)**. "*T*he *Win*dows *C*ontrol and *A*utomation *T*echnology" (TwinCAT) is a suite of software which turns PC-based systems into programmable logic controllers (PLCs) for industrial automation settings. The fact that these "soft" PLCs are PC-based is very powerful, since they can interact directly with the Windows OS, opening them up to possible uses not available with traditional PLCs.

One of DMC's core values with which I'm passionately aligned is **"sharing information."** During my time at DMC, the company's Beckhoff services grew quite a bit in size, and necessarily so did our expertise. I wanted to make sure, as our team of Beckhoff-capable engineers grew, that we likewise expanded our documentation and internal training material to make it easier to bring new members onto that team. I also wanted to contribute what I could to the Beckhoff development community with **public information that could be used by anyone trying to get started with Beckhoff PLC programming**.

For these reasons, I made sure to take note of the topics I wish I knew when I was first learning TwinCAT 3. Using that list, I utilized any extra time I had in between developing for my clients to write internal training material and publish public reference blogs. Towards the end of my tenure with DMC, the training material I contributed to was used to ramp engineers **straight out of university into contributing to client TwinCAT 3 projects within 1 month of starting at the company**. 
Several of **[my public blogs](https://www.dmcinfo.com/latest-thinking/blog/articletype/authorview/authorid/248)** were well received, garnering high time on page and low bounce rates from visitors. 

Below are summaries of my blogs. If you're interested in more info, feel free to click on the titles!

<br/>

#### - [Getting Started With a Beckhoff PLC (3 Part Series)](https://www.dmcinfo.com/latest-thinking/blog/id/10162/getting-started-with-a-beckhoff-plc-part-one--setup)

One of the first things you need to do when you get an actual Beckhoff IPC is configure it to work as a PLC! Sometimes this isn't the most straightforward, so we decided we wanted a step-by-step guide on how to do that. The purpose of this blog series was to be exactly that - at least in the general case where the IPC's OS is not Windows CE or NTSC.


#### - [Version Control/Multi-User Development](https://www.dmcinfo.com/latest-thinking/blog/id/10317/version-control-and-multi-user-development-with-beckhoff-twincat-3) and a [Step-by-Step Guide](https://www.dmcinfo.com/latest-thinking/blog/id/10318/setting-up-a-twincat-3-project-for-version-control-a-step-by-step-guide)

As a software developer, there is not much I love more than *good version control*. It's a critical tool, but the automation industry has been a little slow to catch up in using it. Luckily, TwinCAT 3 file storage is entirely text-based and places heavy emphasis on the [IEC 61131-3 structured text language](https://en.wikipedia.org/wiki/Structured_text).

For these reasons, TwinCAT 3 plays great with version control systems like Git. There's some nuance to it, so I created these companion blogs to discuss tips and tricks and a step-by-step guide for setting up your TwinCAT 3 project for version control.

#### - [Hardware Linking in TwinCAT 3](https://www.dmcinfo.com/latest-thinking/blog/id/10336/how-to-link-hardware-io-in-beckhoff-twincat-3)

Writing controls software and simulating it on your development PC is all well and good (and TwinCAT is great at it!) but as a mechanical engineer and a roboticist, I like to see things move! To do that, you need to link the variables in your software with the real physical hardware inputs and outputs in your system.

TwinCAT has a few ways of doing this, some straightforward and some not so much. So I wrote this blog as a deep dive into the different ways you can link your hardware, and why you might choose a particular method.

#### - [TwinCAT Project Variants](https://www.dmcinfo.com/latest-thinking/blog/id/10299/twincat-project-variants)

Project Variants are a capability unique to TwinCAT in the automation space. They allow you to reconfigure your hardware and even execute different code on your PLC within a single project by just switching an option in a dropdown. This is incredibly powerful for situations like simulation and maintaining multiple machines with similar code. But with any powerful capability also comes complication! I wrote this blog to dive into how, when, and why to use TwinCAT Project Variants.
