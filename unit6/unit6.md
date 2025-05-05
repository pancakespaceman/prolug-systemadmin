# Unit 6 - Firewalls

## Overview

This unit focuses on Nohup environments and firewalls.
	- We will cover Nohup tools and how to properly use Nohup environments.
	- We will explore different types of firewalls and learn the use cases for each firewall type.

## Learning Objectives

1. Become familiar with the `nohup` command:
	- Learn real-life use cases of the `nohup` command.
	- Understand the correlation between jump boxes and Nohup environments, including
	`screen` and `tmux`

2. Implement and manage Nohup environments:
	- Learn how `nohup` allows processes to continue running after a user logs out,
	ensuring that long-running tasks are not interrupted.
	- Develop skills in managing background processes effectively using `nohup`,
	`screen`, and `tmux`

3. Develop effective troubleshooting methodologies:
	- Acquire systematic approaches to diagnosing firewall misconfigurations,
	network connectivity issues, and unauthroized access attempts.
	- Apply structured troublshooting strategies to minimize downtime and maintain
	high availability.

## Prerequisites

- A basic understanding of how processes work.
- Familiarity with the `firewalld` service
- The ability to understand `.xml` files

## Key Terms and Definitions

Firewall
Zone
Service
DMZ (Demilitarized Zone)
Proxy
Stateful Packet Filtering
Stateless Packet Filtering
WAF (Web Application Firewall)
NGFW (Next-Generation Firewall):

## Lecture Notes

### Noup environments

If I disconnect while a process is running, should I see 

Nohup is the idea that the processes we are running will continue to run
even whent the parent process is killed.
	- such as if you ssh into a server, start a process, exit the ssh, 
	and then relog in.

Sometimes connection sessions can have timeouts, so if we want processes
to contiue beyond our sessions we need nohups.


### Firewalls

