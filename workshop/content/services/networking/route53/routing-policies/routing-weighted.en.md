---
title: "Amazon Route 53 - Weighted Routing Policy"
menutitle: "Weighted Routing"
weight: 3
chapter: false
pre: "<b></b>"
awsServices:
 - "Amazon Route 53"
tags: 
 - Warm Standby
type: Documentation
---

Weighted routing allows you to associate multiple resources with a single domain name (example.com) or subdomain (acme.example.com) and choose the amount of traffic routed for each resource. This can be useful for a variety of purposes, including load balancing and testing new versions of software.

To set up weighted routing, create records that have the same name and type as each of your resources. To each record you assign a relative weight that corresponds to the amount of traffic you want to send to each resource. Amazon Route 53 sends traffic to a resource based on the weight you assign to the record as a proportion of the total weight for all records in the group:

![Weighted Routing](/images/route53-weighted.png)
