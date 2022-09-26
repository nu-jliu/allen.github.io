---
layout: post
title: Industrial Automation Consulting
description: PLC, SCADA, HMI, Databases, & More
image: assets/images/industrial-automation/main.jpg
---

From August 2019 to August 2022, I consulted as a part of the automation team at **[DMC, Inc](https://www.dmcinfo.com/)**. Our task was to help our clients with anything related to industrial automation. While we worked with a wide variety of sometimes unexpected technologies, our work usually fell into one of the following categories:

- **PLCs** - **P**rogrammable **L**ogic **C**ontrollers - ruggedized, long-lasting controllers used for real-time control in industrial settings.
- **HMIs** - **H**uman **M**achine **I**nterfaces - (generally) screens and other interfaces that allowed operators and technicians to interact and control their machines.
- **SCADA** - **S**upervisory **C**ontrol **A**nd **D**ata **A**cquisition - (generally) plant level software used for monitoring/controlling a large network of machines and handling the data they produce.

Work at DMC was fast-paced, with project scales generally falling between 2 weeks and 6 months. I often handled projects for multiple clients at a time, designing system architecture, developing software, and travelling to client sites to commission their systems. During my second and third year at the company, I took on technical leadership and project management responsibilities for several projects.


Due to client confidientiality, I can't go into details about the projects I worked on. However, my clients did include companies in the following sectors:
- Electric Car/Battery Manufacturing
- High-Speed Transportation
- Cell Therapy Manufacturing
- Healthcare Diagnostics Manufacturing
- Confectionary Manufacturing
- Metalworking

Throughout these projects I worked with technologies including:
- Beckhoff TwinCAT 3 PLC/HMI*
- Ignition SCADA*
- MS SQL Server
- Aveva Edge (Indusoft)
- Iconics GEN64
- Siemens TIA Portal and SIMATIC Manager
- Siemens WinCC 7.5
- Rockwell Studio 5000 Logix Designer

Technologies marked with an asterisk (*) are the platforms I spent the most time with. Because I worked with so many technologies on such varied projects, I learned how to pick up new platforms and concepts quickly. Some of the general topics I gained experience with include:

- PLC programming/architecture design
- User-friendly interface design
- Scripting (Python often, occasionally VBScript and Javascript)
- PLCOpen motion control
- Object Oriented Programming (OOP)
- Database design
- Networking

Some more detail on some noteable items:

#### PLCOpen motion control

I generally worked with servo motors, developing with [PLCOpen](https://plcopen.org/technical-activities/motion-control) style blocks for point to point motion. Occasionally, for some of the more niche jobs, I worked with more involved motion control, which typically involved using PID to control an motion output (ex: drive velocity) to achieve a desired goal (ex: force exerted on a part).

#### Object Oriented Programming (OOP)

Though [IEC 61131-3](https://en.wikipedia.org/wiki/IEC_61131-3) establishes standards for organization of code in PLC software development, object oriented programming in practice is still pretty new in the industrial automation space. Luckily, [Beckhoff TwinCAT 3](https://www.beckhoff.com/en-us/products/automation/twincat/), a platform with which I worked extensively at DMC, implements object oriented programming concepts well.

Across several projects at DMC I helped architect and guide our OOP development. In order to aid our from-scratch development, I spent time designing and developing function blocks (classes) for PLC control libraries. These included basic control for devices like digital/analog inputs/outputs, servo control, sequencers, and more. I also pioneered the use of [unit testing](https://tcunit.org/) for these PLC control libraries at the company to help ensure quality of the developed libraries.

#### Database design

While I in no way claim to be an expert in database design, I did learn best practices from DMC's application development team. I gained experience in database schema design, efficient query writing, and stored procedure writing so I could better design systems to store data from clients' SCADA systems.