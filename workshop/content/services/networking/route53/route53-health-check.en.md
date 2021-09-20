---
title: "Amazon Route53 - Health Checks"
menutitle: "Health Checks"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
type: documentation
---

Create a *health check*, if you want Amazon Route 53 to check the health status of an address that will be used to respond to DNS queries using this record. This check works as a filter, so only addresses that passed the assessment will be returned, which are healthy.

![Route53 - Health Check](/images/route53-health-checks.png)

{{% notice warning%}}
If you create a *health check* to the DNS record itself and then associate that DNS record with it *health check*, the results of *health check* will be unpredictable.
{{% /notice%}}

{{< youtube vGywoYc_sA8>}}

---
**References**
- [How Amazon Route 53 determines whether a health check is healthy](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-determining-health-of-endpoints.html)
