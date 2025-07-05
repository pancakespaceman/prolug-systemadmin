# Unit 12 - Baselines & Benchmarks

## Overview

In this unit, we focus on baselining, benchmarking, testing methodology, and data 
analytics — all essential skills for Linux system administrators. These topics 
allow us to understand the current state of our systems, measure performance under 
varying loads, and validate improvements with real data.

We’ll explore how to gather accurate system information using tools like iostat, 
sar, stress, and iperf3, and learn how to develop test plans that can support 
decision-making and capacity planning. Whether you're justifying budget increases 
or validating a new storage solution, knowing how to gather and present performance 
data makes you a more effective administrator.


## Learning Objectives

By the end of this unit, you will be able to:

	- Define and use key concepts: baseline, benchmark, high watermark, scope, and methodology
	- Use tools like sar, iostat, stress, and iperf3 to collect performance data
	- Create and execute test plans to evaluate system behavior under different loads
	- Apply data analytics concepts: descriptive, diagnostic, predictive, and prescriptive
	- Communicate system performance clearly with stakeholders through objective evidence

## Relevance and Context

Understanding how your systems behave under normal and stressful conditions is a 
cornerstone of professional Linux administration. In today’s environments, decisions 
about agents, updates, or infrastructure changes require proof — not guesswork.

This unit prepares you to be the voice of data in meetings with architects and 
management. From proving system utilization for budget requests to testing performance 
claims from vendors, these skills help you become a confident, evidence-driven engineer.

## Prerequisites

Before starting Unit 12, you should have:

	- Basic Linux administration skills and terminal comfort
	- Familiarity with resource categories: CPU, memory, disk, and networking
	- Access to a Rocky 9 (or similar) Linux system with sudo or root access
	- Ability to install and use CLI tools (dnf install, rpm, etc.)

## Key Terms and Definitions

Baseline

Benchmark

High Watermark

Scope

Methodology

Testing

Control

Experiment

Analytics

	- Descriptive
	- Diagnostic
	- Predictive
	- Prescriptive


## Lecture Notes

Purpose of Baselining

follows along with observability before to see how the system works under normal
operations. This can be thought of as a "low watermark" of the system. Baselines 
should be capture with telemetry and observability systems so that high and low 
deviations from the norm can be observed.

### Tools and Measurement

The precision of the tool and your ability to use and interpret the tool are the
most important factors to making good assessments and predicting proper outcomes.

- Garbage in, garbage out
	- this applies to any process or system you can think of

Resolution of metrics: how many data points are we collecting? how often?
	- higher resolution means more cost
	
### Intro to Testing

All tests seek to record accurate info about the state of the system. This requires
a good testing methodology to ensure accuracy of information in the scoping, collecting,
storage and aggregation, and controls of that data collection.

Purpose: why are we doing the test?

Scope: how are we conducting the test? what is captured?

Out of Scope: What is not captured? What is acknowledged, but not recorded? Even
if an anomaly occurs, is it something we do not consider?

Methodology: Description of how we will work to objectively determine quality of
the outcomes. Are we using qualitative or quantitative results? What normalization
are we using (percentage, other)?

Control: What will be put in place to ensure that collection is done against a
similar environment between different tests?


### Intro to Benchmarking

Benchmarking Iteration

Plan phase:
	1. Goals identification
	2. Tools and metrics identification
	3. Planning and resource allocation

Experiment Phase:
	1. Experiment definition
	2. Experiment execution
	3. Experiment Result Analysis

Improve Phase:
	1. Benchmark report
	2. Improvement planning
	3. Improvement monitor

### Data analytics

Descriptive analysis
	- what happened in the past
Diagnostic analytics
	- why did it happen in the past
predictive analytics
	- what will happen in the future
prescreptive analytics
	- how can we make it happen (or not happen)

### Why do we care as an Admin?	

Because we are the ones that see the system in operations. We have to understand
how soething works so we can deal with how to appropriate scope and scale future
versions of the system.

- We measure and analyze the system in the running state. Sometimes we're the first
people to have see a new system in operation.
- Ceteris Paribus - Latin, typically an economic term. It means all other being
held equal. We try to limit any non-dependent variables from being important in our
evaluations.
	- give our tests a fair chance, do stuff the same way against the same type
	of variables
- Last week we talked about "What we monitor we can improve" ~ Demin
- We can start to predict how the system will behave if certain conditions that
we monitor occur.

### Places to get data to practice with

Data sites
https://data.gov/
https://www.kaggle.com/

Places to practice with data:
https://www.kaggle.com/
https://www.noaa.gov/information-technology/open-data-dissemination

