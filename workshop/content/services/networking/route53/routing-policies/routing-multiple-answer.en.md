---
title: "Amazon Route53 - Multi-Response Routing Policy"
menutitle: "Multi-Response"
weight: 4
chapter: false
pre: "<b></b>"
awsServices:
  - "Amazon Route53"
tags: 
 - Active-Active
type: Documentation
---


Multi-value response routing allows you to configure Amazon Route 53 to return multiple values, such as IP addresses of your web servers, in response to DNS queries. You can specify multiple values for virtually any record, but multiple-value response routing also allows you to verify the integrity of each resource so that Route 53 returns only values for healthy resources. This type of routing does not replace the load balancer. However, the ability to return multiple IP addresses whose integrity can be verified is a way to use DNS to improve availability and load balancing.

To route traffic roughly and randomly to multiple resources, such as web servers, you create a multi-value response record for each resource and optionally associate a Route 53 health check with each record. Route 53 responds to DNS queries with up to eight healthy records and provides different responses to different DNS resolvers. If a web server becomes unavailable after a resolver caches a response, the client software may try another IP address in the response.

Notice the following:

*   If you associate a health check with the multi-value response record, Route 53 responds to DNS queries with the corresponding IP address only when the health check is complete.

*   If you don't associate a health check with a multi-value response record, Route 53 always considers the record to be healthy.

*   If you have eight or fewer healthy records, Route 53 responds to all DNS queries with all healthy records.

*   When none of the records are healthy, Route 53 will respond to DNS queries with up to eight records with health issues.

---
**References**
- [Multivalue Answer Routing](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-multivalue)
