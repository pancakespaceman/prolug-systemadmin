# Unit 11 - Monitoring

## Overview

In this unit, we focus on Linux system monitoring, using modern tools like Grafana,
Prometheus, Node Exporter, and Loki. As Linux administrations, monitoring is essential
to ensure system stabilty, performance, and security across environments.

We will explore how to collect, analyze, and visualize system metrics, and discuss
best practices for monitoring and dashboard design that can improve troubleshooting
and proactive system management.

## Learning Objectives

By the end of this unit, you will be able to:

- Explain core monitoring concepts like metrics, logs, SLOs, SLIs, and KPIs
- Set up Prometheus and Node Exporter to collect system metrics
- Use Grafana to create dashboards for visualizing system health and performance
- Write and execute PromQL queries to analyze system data
- Interpret monitoring data to diagnose system issues and support teams with actionable
insights

## Relevance & Context

Monitoring is a *core responsibility* of Linux system administration, ensuring you
know what's happening under the hood before issues escalate. Modern IT environments
rely on monitoring to track system performance, security events, and overall stability
- whether in production, development, or cloud environments.

This unit focuses on Grafana for visualization and Prometheus with Node Exporter
for telemetry and metrics collection -- tools used in enterprise, cloud, and HPC
(High-Performance Computing) environments.

Whether you're in a NOC, SysAdmin, or DevOps role, understanding monitoring and
telemetry makes you a key contributor to system reliability and performance.


## Prerequisites

Before starting Unit 11, you should have:

- Basic understanding of Linux system administration and networking
- Familiarity with system processes, performance metrics, and logs
- Root or sudo access to a Linux system (Rocky 9 or equivalent)
- Internet access to run labs via Killercoda and online resources
- (Optional but recommended): Exposure to containers and services like Grafana
or Prometheus

## Key Terms and Definitions

SLO (Service Level Objective)

SLA (Service Level Agreement)

SLI (Service Level Indicator)

KPI (Key Performance Indicator)

MTTD (Mean Time to Detect)

MTTR (Mean Time to Repair)

## Lecture Notes

Monitoring Topics:
	- What we monitor
	- How we monitor
	- How we present the data
		- How we build dashboards
		- Who we build dashboards for

What types of data

- Logs: A record of what is happening within your system or software
	- should be timestamped, what system
- Metrics: A numberical assessment of system or application performance and
resource utilization
- Traces: How data is moving through your system. Also checkpoints

### From a sys admin's perspective

When we start having issues, for every resource check:

- Utilization: resource busy time
- Saturation: queue length, queued time
- Errors: errors reported in logs, they should be easy to spot

Everything here is easier if you're actively monitoring your system


How do we check a server out all times of the day? Its easy to do current
snapshots with commands like fre

### Why do we care
Because good visibility into the system is the number one thing we can use to
predict and prevent errors.

- we have to have our own view of the system
- we often have to provide views for others who have differnent goals
	- management
	- monitoring teams or 24/7 NOC/SOC
- we may be called on to build out or put together all of these monitoring systems

BIG 4

1. CPU
2. Memory
3. Disk Usage
4. Network

### Example layouts to look at

What information do you think they’re trying to convey?
What is good about them?
What do you think is a weakness of them?

https://grafana.com/api/dashboards/14981/images/11093/image
https://grafana.com/api/dashboards/14900/images/10916/image
https://grafana.com/api/dashboards/593/images/6899/image
https://grafana.com/api/dashboards/15661/images/11580/image


### Projects channel

there are some monitoring deep dive in power points in the projects channel



