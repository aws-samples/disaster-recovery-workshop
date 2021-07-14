---
title: "Amazon Route53 - Simple Routing Policy"
menutitle: "Simple"
weight: 3
chapter: false
pre: "<b></b>"
awsServices:
  - "Amazon Route53"
tags: 
 - Active-Active
type: Documentation
---


Simple routing allows you to configure standard DNS records without special Route 53 routing, such as weighted or latency. With simple routing, you typically route traffic to a single resource, for example, to a web server on your site.

If you choose the simple routing policy in the Route 53 console, you cannot create multiple records with the same name and type, but you can specify multiple values in the same record, such as multiple IP addresses. (If you choose the simple routing policy for an alternative record, you can only specify an AWS resource or a record in the current hosted zone.) If you specify multiple values in a record, Route 53 displays all the values to resolve it recursive in random order and the resolver displays the values for the client (such as a web browser) that submitted the DNS query. The customer then chooses an amount and resends the query.

---
**References**
- [Amazon Route53 - Simple Routing](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-simple)
