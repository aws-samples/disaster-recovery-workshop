---
title: "Availability"
menutitle: "Availability"
weight: 1
chapter: false
pre: "<b></b>"
type: Documentation
---

Availability (also known as service availability) is a metric commonly used to quantitatively measure reliability.

* Availability is the percentage of time a workload is available for use.

This percentage is calculated in periods such as a month, a year, or the last three years. Applying the strictest possible interpretation, availability decreases whenever the application is not operating normally, including scheduled and unscheduled outages.

{{< youtube Pr7u2dSKS38>}}

The following is a table of common application availability design goals and the maximum duration of outages that can occur in a year without the goal being reached. The table contains examples of common application types at each availability level.

|Availability Objective|Max. Unavailability (by year)|RTO|# of Regions|Application Type
|--|--|--|--|--|
|90%|36 days and 12 hours|4 - 24 hours|One|.|
| 95% |18 days and 6 hours|4 - 12 hours|One|.|
|[99%](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/s-99-scenario.html)|3 days and 15 hours|2 - 8 hours|One|batch processing tasks, data analysis|
|[99.9%](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/s-99.9-scenario.html)|8 hours 45 minutes|1 - 4 hours|One|a company's internal tools|
|[99.95%](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/s-99.95-with-a-recovery-time-between-5-and-30-minutes.html)|4 hours 22 minutes|30 - 120 minutes|One or more|Online trade, point of sales|
|[99.99%](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/s-99.99-scenario.html)|52 minutes|5 - 20 minutes|Two or more|Video delivery|
|[99.999%](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/s-99.999-or-higher-scenario-with-a-recovery-time-under-1-minute.html)|5 minutes|1 - 60 seconds|Two or more|Financial Transactions|

{{% notice info%}}
Use the table above as a reference to create the version tailored to the needs of your applications. Regardless of the recovery strategy chosen, it cannot exceed the Recovery Timeout (RTO).
{{% /notice%}} <br>


---
**References**

- [AWS Well-Architected Framework - Availability](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/availability.html)
- [Failure is not an Option - Designing Highly Resilient AWS Systems](https://www.youtube.com/watch?v=Pr7u2dSKS38)
