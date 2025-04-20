# Unit 4 - Operating Running Systems

## Overview

This unit concentrates on the core tasks involved in operating running sytems
in a Linux environment, particularly with Red Hat Enterprise Linux (RHEL)

It covers:
  - Understanding resource usage CPU, memory, disk I/O
  - Becoming familiar with service management frameworks

## Learning Objectives

1. Monitor and Manage System Requirements
  - learn to track CPU, memory, disk, and network usage.
  - Understand how to troubleshoot performance bottlenecks.
2. Master Service and Process Control
  - Gain proficiency with systemd for managing services and understanding
  dependency trees.
  - Acquire the ability to identify, start, stop, and restart services and
  processes as needed.
3. Implement Scheduling and Automation
  - use `cron`, `at`, and `systemd` timers to automate recurring tasks.
  - Understand how automated job scheduling improves reliability and reduces
  manual intervention.
4. Monitor and Manage System Resources
  - Leard to track CPU, memory, disk, and network usage using commong RHEL utilities.
  - Understand best practices for troubleshooting performace bottlenecks.
5. Master Service and Process Control
  - Gain proficiency with `systemd` for managing services and understanding dependency
  trees.
  - Acquire the ability to identify, start, stop, and restart services and
  processes as needed.
6. Configure and Interpret System Logs
  - Explore journald and syslog-based logging to collect and store vital system events.
  - Develop techniques to analyze log files for troubleshooting and security assessments
7. Implement Scheduing and Automation
  - use `cron`, `at`, and `systemd` timers to automate recurring tasks.
  - Understand how automated job scheduling improves reliability and reduces
  manual intervention.

These objectives ensure learners can sustain, troubleshoot, and improve
actively running Linux systems within enterprise environments, reducing downtime
and increasing system reliability.

## Relevance & Context

Operating running systesm is central to any Linux administrator's responsibilities
for several reasons:
  - System Stability and Performance
    - continuous monitoring and immediate remediation of issues ensure critical
    sevices remain available and performant.
  - Proactive Problem Resolution
    - Effective log management and automation allow administrators to detect
    anomalies early, schedule essential maintenance, and minimize disruptions.
  - Security and Compliance
    - Logs are often the first line of evidenc ein security auditing and breach
    investigations. Regularly reviewing and correlating logs is crucial to
    maintaining a secure environment.
  - Enterprise Uptime and Reliability
    - In production environments, even brief outages can lead to significant
    operational and financial impacts. Proper management of running systems
    ensures high availability and robust service delivery.

## Key Terms and Definitions

Systemd
Journalctl
Cron / At / Systemd Timers
Daemon

## Lecture Notes


### This Week

Operations Bridge
  - what is it?
  - what does it mean to have a team that handles operations 24/7

REVIEW THE FINAL PROJECT

### Topics

Operating Running sytems

Handling incidents
  - do we have an incident bridge
  - tracking events and timelines
    - what information do we track?
      - time we were alerted
      - what we checked
      - what actions we took
      - did we change anything?
      - were we instructed to change anything?
Configuration Drift
  - 100 servers start the same, but a few weeks later some change, then more
Get stuff in writing to confirm.


### Operating a running system

Gathering system info
  - general system info
    - cat `/etc/*release`
  - disk that exist and file systems
    - `fdisk -l`
    - `df -h`
    - "hot disks", where one disk  is being used instead of the raid
  - system usage stats
    - `iostat`
    - `iostat -xe`
    - `vmstat`
  - Memory
    - `free -m`
  - CPU
    - `nproc`
    - `cpuinfo`
  - Security
    - logs
      - `tail -f /var/log/messages`
      - `tail -f /var/log/secure`
    - open ports
      - `ss -ntulp`
      - `0.0.0.XX` or `*:` means any ipv4 can connect
      - `[::]` is the any for ipv6
    - processes
      - `ps -ef`
      - `ps -ef | awk '{print $1}' | sort | uniq`

### Incident response lifecycle

Security incidents will always be confidentiality, availability or integrity

1. Detection
2. Response
3. Mitigation
4. Reporting
5. Recovery
6. Remediation
7. Lessons Learned

- Reponse capability (policy, procedures, a team)
- Incident response and handling (triage, investigationk containment, and analysis
& tracking)
- recovery (recovery/repair)
- debriefing/feedback (external communication)
- mitigation - limit the effect or scope of an incident


Operations bridge call, everyone quiet
  - if nobody asks in the call, set a timer for vocalizing your progress.
MTTD - averaage time to detect a problem
  - if one person has 5 minute MTTR but everyone else is 55 min
You'll become more efficient in each area the more you learn from each incident.


### What are you doing 

always got a notepad open, taking notes while working along aise a terminal


You get alerted
  - note when you are alerted
  - note acknowledge
check and test assumptions
  - the server can do x
  - the server cannot do x
Document anomalies
  - 8:10 - i found the server unable to ping out to google, but able to ping
  its gateway
  - capture output
Report to bridge
  - as anomalies are found
  - at set intervals

`uptime`, `ping www.google.com`, `systemctl --failed`

There is one database you must shutdown properly in the case of power outage
and you're running backup power.
  - active directory


