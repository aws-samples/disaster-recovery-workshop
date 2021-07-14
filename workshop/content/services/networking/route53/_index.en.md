---
title: "Amazon Route53"
menutitle: "Amazon Route53"
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
type: Documentation
---

Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. You can use Route 53 to perform three main functions in any combination: domain registration, DNS routing, and health check. If you choose to use Route 53 for all three functions, perform the steps in this order:

**1. Register domain names**

Your site needs a name, such as example.com. Route 53 allows you to register a name for your website or web application, known as a domain name.

**2. Route Internet traffic to your domain resources**

When a user opens a web browser and informs their domain name (example.com) or subdomain name (acme.example.com) in the address bar, Route 53 helps connect the browser to the website or web application.

**3. Check the health of your resources (*Health Checks*)**

Route 53 sends automatic requests over the Internet to a resource, such as a web server, to verify that it is accessible, available, and functional. You can also choose to receive notifications when a feature becomes unavailable and choose to divert Internet traffic from unhealthy resources.

---
**References**

- [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html)
