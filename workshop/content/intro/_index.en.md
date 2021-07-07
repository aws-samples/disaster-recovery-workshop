---
title: "Introduction"
weight: 10
chapter: true
pre: "<b>1. </b>"
---

This material discusses software architecture fundamentals and mechanisms for building highly resilient applications in the Amazon Web Services (AWS) environment. The object is to meet the requirements for recovering applications in different layers, including for mission-critical applications.

Resilient applications provide continuous service despite disruptions. Events such as natural disasters, hardware failures, and human errors can cause disruptions to service. Companies that don’t prepare their applications for these failures have a higher risk of downtime, instability, and loss of data in your solutions. This unavailability can lead to financial loss, legal implications, negative impacts on the brand, and customer dissatisfaction.<sup>[1](https://d1.awsstatic.com/Industries/Financial%20Services/Overview/Resilient%20Applications%20on%20AWS%20for%20Financial%20Services.pdf)</sup>

Developing an application considering possible failures is one of the most important principles to make it resilient.
According to the criticality of each application, expected RTO and RPO should be stipulated.
It is important to discuss the [Disaster Recovery Strategies](disaster-recovery) in the AWS environment.

To understand how the AWS infrastructure works, see [Global Infrastructure](global-infra).
If it's your first contact with the AWS environment, see also. [Regions and Availability Zones](regions-az).


---
**References:**
1. [Building Mission-Critical Financial Services Applications on AWS](https://d1.awsstatic.com/Industries/Financial%20Services/Overview/Resilient%20Applications%20on%20AWS%20for%20Financial%20Services.pdf)
