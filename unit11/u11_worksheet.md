# Unit 11 Worksheet - Monitoring

## Resources / Important Links

(How to easily monitor your linux server | Grafana labs)[https://grafana.com/solutions/linux-node/monitor/]
(30 Linux system monitoring tools every sysadmin should know)[https://www.cyberciti.biz/tips/top-linux-monitoring-tools.html]
(monitoring linux using SNMP - Nagios)[https://www.nagios.com/solutions/linux-monitoring/]


## Discussion Posts

### 1

```
You’ve heard the term “loose coupling” thrown around the office about a new monitoring
 solution coming down the pike. You find a good resource and read the section on 
 “Prefer Loose Coupling” https://sre.google/workbook/monitoring/.
```

#### Notes

Monitoring can include many types of data:
	- metrics
	- text logging
	- structured event logging
	- distributed tracing
	- event introspection
	- metrics and structured logging are best suited for SRE's monitoring needs

Monitoring allows you to gain visibility into a system.
You need to understand and prioritize the features that matter for your use case.
Monitoring System attributes:
	- Speed
		- freshness of data and speed of data retrieval
			- how long will it take your monitoring system to page you
			when something goes wrong
			- slow data might lead you to accidentally act on incorrect data
		- mostly a problem when querying vast amounts of data
	- Calculations
		- retain data over a multimonth time frame
			- long-term view of data is needed to analyze long-term trends
		- Summary data (aggregated) can help with growth planning.
		- retaining all detailed individual metrics helps with exposing anomalies.
		- Metrics retained about events or resource consumption should be
		`monotonically incrementing counters`.
	- Interfaces
		- dashboards are primary intercaces for displaying monitoring
			- choose formats that most clearly display the data you care about
			- examples: heatmaps, histograms, and logarithmic scale graphs
		- likely need different views of the same data depending on intended audience.
	- Alerts
		- Classify alerts: multiple categories of alerts allow for proportional responses.
			- severity levels
		- Alert suppresion helps reduce noise for on-call engineers
			- e.g. if all nodes experiencing the same high rate of errors, only alert
			once for the global error rate instead of individual nodes

Sources of Monitoring Data
	- Logs
		- append-only records of events.
		- will have inherent delay between when an event occurs and when its visible
		in the logs
			- can be helpful for analysis that is not time sensitive
			- pro is that information can be very granular
	- Metrics
		- numerical measurements representing attributes and events.
		- data less granular, but near-real time. good for alerts and dashboards.
	- others like distributed tracing and runtime introspection

Managing Monitoring System
	- Treat configuration as code
		- store in revision control systems
	- Encourage consistency
		- centralized approach provides consistency, but individual teams may want
		full control over design of their configuration.
		- depends on org.
		- make basic monitoring coverage effortless.
			- if all services export consistent set of basic metrics, its easier
			to automatically collect metrics across the org. any new service
			automatically has basic monitoring.
	- Prefer loose coupling
		- requirements change over time, same with systems.
		- keep components of monitoring system loosely coupled.
			- stable interfaces for configuring each component and passing monitoring data
			- separate components should be in charge of collecting, storing, alerting,
			and visualizating your monitoring.
			- easier to swap out any given component if a better alternative is needed.
		- a separet dashboarding system that can use multiple data sources provides
		a central and unified overview of your service.

1. What does "loose coupling" mean, if you had to summarize to your junior team members?

```
Think of a restaurant kitchen. The chef knows how to do everything needed to run 
the kitchen, such as prep ingredients, cook food to order, clean dishes, and more.
However, if the chef does everything himself, he may be able to get customers served
but thats a lot of different tasks to handle at once. If one task backs up, then the
rest of the tasks do as well.

So the chef hires workers to perform the different tasks. Now, each worker is 
responsible for a smaller amount of work, such as cooking food to order or washing dishes.
The chef now just needs to check in with the different workers to make sure things
are running smoothly. If one of the workers moves on to a new job, the chef can hire
a new worker to take their place.

Now, instead of a kitchen, now we think of a monitoring system. There are many
different components, or from our analogy diferent tasks, that make up the entire system.
Collecting data, storage, dashboards, and more. We could have a single service managing everything,
but thats like the chef working by himself in the kitchen. Instead, we use different
services for different components, like how the chef hires workers. Components can
be replaced with alternatives as needed, and one component having issues doesn't
stop the others from doing their jobs.
```

2. What is the advantage given for why you might want to implement this type of tooling
in your monitoring? Do you agree? Why or why not?

```
There are a few advantages. Having separate components allows you to more easily
swap out individual components for alternatives as need arises. Also, if a component
fails, or the system that is being monitored fails, the monitoring system as a whole
still functions. Another pro mentioend is that, due to having the dashboard component
be separate from the rest of the monitoring system, it allows users of the dashboard
to be unaffected as the other components were migrated to different alteratives.

These are all great reasons to loosely couple your monitoring systems. Based on my
current understanding, I do think these pros are worthwhile considerations.
However, it truely depends on your use case. A home lab probably does not need
as expansive a loosely coupled monitoring system as Google.
```

3. They mention "exposing metrics", what does it mean to expose metrics? What happens
to metrics that are exposed but never collected?

```
Metrics are exposed when they are made available to be collected and viewed.
If these metrics are never collected, then they are lost when the process restarts,
as they are not stored long-term.
```


### 2

```
Your HPC team is asking for more information about how CPU0 is behaving on a set
of servers. Your team has node exporter writing data out to Prometheus 
(Use this to simulate https://promlabs.com/promql-cheat-sheet/).
```

1. Can you see the usage of CPU0 and what is the query?

```
node_cpu_seconds_total{cpu="0"}
```

2. Can you see to usage of CPU0 for just the last 5 minutes and what is they query?

```
node_cpu_seconds_total{cpu="0"}[5m]
```

3. You know that CPU0 is exluded from Slurm, and you exclude that and only pull
the user and system for the remaining CPUs and what is the query?

```
node_cpu_seconds_total{cpu!="0",mode=~"user|system"}
```

### Responses

[x] Post 1
[x] Post 2


## Definitions

SLO (Service Level Objective)
	- A measureable target for a specific service level indicator, such as "99.9%
	of requests served within 100ms over 30 days." SLOs help teams quantify desired
	reliability.
	- https://grafana.com/docs/tempo/latest/introduction/solutions-with-traces/traces-app-insights
	
SLA (Service Level Agreement)
	- A formal agreement between a provider and consumer specifying expectations
	(uptime, response time, penalties). SLAs are typically backed by SLOs.
	- 

SLI (Service Level Indicator) / KPI (Key Performance Indicator)
	- SLI: A metric that tracks service health (e.g. request latency, error rate)
	- KPI: A broader term used across business tech to denote any measurable value
	indicating performance.

Span
	- In distributed tracing, a span represents a single operation (e.g. HTTP request,
	DB query), recording start/end time and metadata. Multiple spans form a trace,
	showing the journey of a request through services.

Trace
	- A collection of spans across multiple systems, used to understand the path and
	timing of a request. Tracing helps identify bottlenecks in distributed systems.

Prometheus
	- An open-source monitoring and alerting toolkit that scrapes metrics via HTTP
	endpoints and stores time-series data. It supports complex queries using PromQL
	and is the foundation for many observability stacks.

Node_Exporter
	- An agent providing hardware and OS-level metrics (CPU, memory, disk, network)
	to Prometheus by exposing `/metrics`.

Grafana
	- A popular open-source visualization and dashboard tool used to display metrics
	from data sources like Prometheus, with support for alerts and annotaions.

Dashboard
	- A collection of visualizations (graphs, tables, stats) tailored to display
	insights about system or application health--custom built using Grafana, Kibana, etc.

Heads up Display
	- In monitoring, it refers to a real-time, glanceable display showing key metrics
	without the need to navigate complex dashboards--akin to a cockpit HUD.

## Digging Deeper

1. Read the rest of the chapter https://sre.google/workbook/monitoring/ and note 
anything else of interest when it comes to monitoring and dashboarding.

2. Look up the “ProLUG Prometheus Certified Associate Prep 2024” in 
Resources -> Presentations in our ProLUG Discord. Study that for a deep dive 
into Prometheus.

3. Complete the project section of “Monitoring Deep Dive Project Guide” from the 
prolug-projects section of the Discord. We have a Youtube video on that project as well. 
https://www.youtube.com/watch?v=54VgGHr99Qg

## Reflection Questions

1. What questions do you still have about this week?

2. How can you apply this now in you current role in IT? If you're not in IT, how can
you look to put something like this into your resume or portfolio?


