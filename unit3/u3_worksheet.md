# Unit 3 Worksheet - Storage

## Recourses / Important Links

(Google SRE Book - Implementing SLOs)[https://sre.google/workbook/implementing-slos/]
(AWS High Availability Architecture Guide)[https://docs.aws.amazon.com/pdfs/whitepapers/latest/real-time-communication-on-aws/real-time-communication-on-aws.pdf]
(Red Hat High Availability Cluster Configuration)[https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_high_availability_clusters/index]

## Discussion Posts

### 1

Scan the chapter (here)[https://google.github.io/building-secure-and-reliable-systems/raw/ch17.html]
for keywords and pull out what you think will help you to better understand how to
triage an incident.

Read the section called "Operation Security" in this same chapter.

1. What important concepts do you learn about how we behave during an operational
response to an incident?

```
Out of everything I've read, my takeaway for the most important thing is that
planning outweighs immediate action. Allowing emotions to flair and cause panic
will cause you to make mistakes and it can cause more harm than good. That extra
5 minutes of planning before can be a big difference in the outcome of the
incident response.

Triaging is an important step, where facts are gathered to assess what is the
situation. Is it an error? a simple problem? or maybe something complex and
potentially damaing? 

Having a team and explicit communication is very important as well. An Incident
Commander should be chosen to organize the response team. Depending on the
scale of the team needed for the response, other leaders can be chosen such as
an Operations Lead, a Management Liaison, a Lead Lead, and a Communications Lead.
The IC should have regular communication with team members to ensure progress
is being made. Communication should be explicit, as misunderstandings can lead
to major issues.

Operational Security is a big factor for the majority of incidents. Keeping
response activity secret ensures the attack is not notified they've been detected.
If the attack does learn they are detected, it can lead to countermeasures
that may even include destroying the org on their way out.

With any incident, its important that we focus, not on making the _right_ call,
but the _best possible_ call out of many suboptimal options.
```

### 2 

Ask Google, find a blog, or ask an AI about high availability.
(AWS Real-Time Communication Whitepaper)[https://docs.aws.amazon.com/pdfs/whitepapers/latest/real-time-communication-on-aws/real-time-communication-on-aws.pdf#high-availability-and-scalability-on-aws]

1. What are some important terms you read about? Why do you think understanding HA
will help you better in the context of triaging incidents?

```
Important terms:
  - Five 9's: a system that is operational 99.999% of the time annually, which
  means there's roughly only 5 minutes and 15 secons of downtime per year.
  - Single Point of Failure: a component, such as a server, that , if it fails,
  brings down the entire system or service. Systems should be designed to avoid
  SPOFs.
  - Active-Standby: Where there are two systems, one active and one standby. The
  standby system is ready to take over the the active system fails.
  - Scalable, load balanced cluster: group of interconnected computers designed
  to work together as a single system to handle increased demand. Load balancing
  means that network traffic or workload is distributed across multiple computers.
  Scalable means the system can handle increased workloads by scaling out
  horizontally (adding more machines) or vertically (adding more resources to
  existing machines).

Understanding HA means more than just knowing these terms. It means that systems
are designed and setup with failure in mind. Redundancies put in place, such as
active-standby configs, allow for systems to remain up while incidents are triaged.
Properly setup systems also mean there should be proper documentation, response
procedures, and monitoring in place to allow for triaging and incident response,
and monitoring in place to allow for smooth triaging and incident response.
```

## Definitions

Five 9's:
  - A measure of high availability, indicating that a system is operational 99.999%
  of the time annually, roughly 5 minutes and 15 seconds of downtime per year.
  - Used for: Mission-critical services (e.g. medical, finance, telecom)
  - (Splunk Five 9's)[https://www.splunk.com/en_us/blog/learn/five-nines-availability.html]

Single Point of Failure (SPOF):
  - A component (e.g. server, switch, database) that, if it fails, brings down
  the entire system or service
  - Goal is to eliminate SPOFs to improve redundancy and reliability.
  - (wiki)[https://en.wikipedia.org/wiki/Single_point_of_failure]

Key Performance Indicators (KPIs):
  - Measurable values that demonstrate how effectively a service or team is
  achieving key objectives.
  - Examples: Uptime %, latency, ticket resolution time.
  - (KPIs)[https://www.kpi.org/kpi-basics]

Service Level Indicator (SLI):
  - a specific metric that quantifies the performance of a service, such as latency
  or error rate.
  - used to asses whether a service meets it expected performance levels
  - (sumo logic SLI)[https://www.sumologic.com/glossary/sli-service-level-indicator]

Service Level Objective (SLO):
  - a target value or range of values for a service level that is measured by an SLI.
  - defined the acceptable performance level for a service of a specific period
  - (google SLO)[https://sre.google/sre-book/service-level-objectives]

Service Level Agreement (SLA):
  - a formal agreement between a service provider and a customer that outlines the
  expect level of service, including metrics like uptime and response time, and
  the consequences if these are not met.
  - (CID)[https://www.cio.com/article/274740/outsourcing-sla-definitions-and-solutions.html]

Active-Standby:
  - a configuration where one system (active) handles all the workload while
  the other (standby) remains idle until the active system fails, at which
  point the standby takes over.
  - (keysight)[https://www.keysight.com/blogs/en/tech/nwvs/2020/06/10/active-vs-standy-different-approaches-for-failover]

Active-Active:
  - a configuration that involves multiple systems running concurrently, sharing
  the load and providing redundancy. If one system fails, the others continue
  to handle the workload without interruption.
  - (jscape)[https://www.jscape.com/blog/active-active-vs-active-passive-high-availability-cluster]

Mean Time to Detect (MTTD):
  - the average time it takes to detect an issue or failure in a system. A lower
  MTTD indicates quicker detection, which can lead to faster resolution.
  - (splunk)[https://www.splunk.com/en_us/blog/learn/mean-time-to-detect-mttd.html]

Mean Time to Recover/Restore (MTTR):
  - measures the average time required to recover from a system failure or outage.
  - (Fiix)[https://fiixsoftware.com/maintenance-metrics/what-is-mean-time-to-recovery/]

Mean Time between Failures (MTBF):
  - the predicted elapsed time between inherent failures of a system during operation.
  - (Wiki)[https://en.wikipedia.org/wiki/Mean_time_between_failures]

## Digging Deeper

1. If uptime is so important to us, why is it so important to use to also understand
how our systems can fail? Why would we focus on the thing that does not drive uptime?

Its exacty the thing that drives uptime. If you don't know how a system can fail,
you wont know how to prevent those failures.1

2. Start reading about SLOs: (Implementing SLOs)[https://sre.google/workbook/implementing-slos/]
How does this help you operationally? Does it make sense that keeping systems within
defined parameters will help keep them operating longer?


## Reflection Questions

1. What questions do you still have about this week?

2. How are you going to use what you've learned in your current role?


