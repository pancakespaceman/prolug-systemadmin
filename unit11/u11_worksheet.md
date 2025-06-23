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

1. What does "loosing coupling" mean, if you had to summarize to your junior team members?

```

```

2. What is the advantage given for why you might want to implement this type of tooling
in your monitoring? Do you agree? Why or why not?

```

```

3. They mention "exposing metrics", what does it mean to expose metrics? Why happens
to metrics that are exposed but never collected?

```

```


### 2

```
Your HPC team is asking for more information about how CPU0 is behaving on a set
of servers. Your team has node exporter writing data out to Prometheus 
(Use this to simulate https://promlabs.com/promql-cheat-sheet/).
```

1. Can you see the usage of CPU0 and what is the query?

```

```

2. Can you see to usage of CPU0 for just the last 5 minutes and what is they query?

```

```

3. You know that CPU0 is exluded from Slurm, and you exclude that and only pull
the user and system for the remaining CPUs and what is the query?

```

```

### Responses

[] Post 1
[] Post 2


## Definitions

SLO

SLA

SLIKPI

Span

Trace

Prometheus

Node_Exporter

Grafana

Dashboard

Heads up Display


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


