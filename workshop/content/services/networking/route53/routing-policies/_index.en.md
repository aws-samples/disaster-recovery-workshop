---
title: "Amazon Route53 - Routing Policies"
menutitle: "Routing Policies"
weight: 3
chapter: true
pre: "<b></b>"
awsServices:
 - "Amazon Route53"
type: Documentation
---

When you create a record, you can choose a routing policy, which determines how Amazon Route 53 responds to queries:

**Simple routing policy** — Use for a single resource that performs a certain role for your domain, for example, a web server that provides content to the example.com site.

**Failover routing policy** — Use when you want to configure active-passive failover.

**Geolocation routing policy** — Use when you want to route traffic based on the location of users.

**Geo-proximity routing policy** — Use when you want to route traffic based on the location of your resources and optionally switch resource traffic at one location to resources elsewhere.

**Latency Routing Policy** — Use when you have resources across multiple AWS regions and want to route traffic to the region that provides the best latency.

**Multi-Value Response Routing Policy** — Use when you want Route 53 to respond to DNS queries with up to eight randomly selected healthy records.

**Weighted routing policy** — Use to route traffic to multiple resources in the proportions you specify.

{{% notice tip%}} <i class="fas fa-lightbulb"></i>
The choice of routing policy should follow the disaster recovery strategy being implemented. See the suggestion table below:
{{% /notice%}} 

| Politics | Strategy |
|-|-|
| [Simple (Multiple IP's)](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-simple) <br> [Weighted (50%-50%)](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-weighted) <br> [Multiple Values](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-multivalue) | Active-Active |
| [Weighted](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-weighted) | Warm Standby |
| [Failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-failover) | Pilot Light|

<br>

{{< youtube WDDkLOT8SCk>}}

---
**References:**
- [Amazon Route53 - Routing Policy](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html)
- [Amazon Builders'Library - Workload Isolation Using Random Fragmentation Order](https://aws.amazon.com/builders-library/workload-isolation-using-shuffle-sharding/)
