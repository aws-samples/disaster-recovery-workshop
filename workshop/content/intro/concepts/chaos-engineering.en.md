---
title: "Chaos Engineering"
menutitle: "Chaos Engineering"
weight: 2
chapter: false
pre: "<b></b>"
type: documentation
---

Chaos Engineering is the discipline of experimenting with a system with the aim of increasing confidence in its ability to withstand problems in your environment.

Testing in non-productive environments should be performed regularly and should be part of the CI/CD cycle. In production, teams must perform the tests in such a way as not to cause the service to be unavailable.

Test results should be measured and then compared with availability objectives to understand whether the application running in a particular environment is able to achieve the defined objectives.

{{< youtube ztiPjey2rfY >}}

---
### AWS Fault Injection Simulator
AWS FIS is a service dedicated to simulating failures in the AWS environment so that testing can be carried out in a controlled manner. AWS FIS simplifies the process of controlling the experimentation of applications in a crash environment. For more information, see: [AWS Fault Injection Simulator](https://aws.amazon.com/fis/)

---
**References:**

- [Chaos Engineering Principles](https://principlesofchaos.org/pt)
