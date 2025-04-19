# Unit 3 - Storage

## Overview

This unit focuess on understanding and implementing techniques to ensure systems remain 
operational with minimal downtime.
  - The process of quickly assessing, prioritizing, and addressing system incidents.
  - Leveraging performance indicators (KPIs, SLIs) and setting clear operational
  targets (SLOs, SLAs) to guide troubleshooting and recovery efforts.


## Learning Objectives

1. Understand Fundamental Concepts of System Reliability and High Availability:
  - Explain the importance of uptime and the implications of “Five 9’s” availability in mission-critical environments.
  - Define key terms such as Single Point of Failure (SPOF), Mean Time to Detect (MTTD),
  Mean Time to Recover (MTTR), and Mean Time Between Failures (MTBF).

2. Identify and Apply High Availability Architectures:
  - Differentiate between Active-Active and Active-Standby configurations and describe 
  their advantages and trade-offs.
  - Evaluate real-world scenarios to determine where redundancy and clustering 
  (using tools like Pacemaker and Corosync) can improve system resilience.

3. Develop Incident Triage and Response Skills:
  - Outline a structured approach to incident detection, prioritization, and resolution.
  - Use performance metrics (KPIs, SLIs, SLOs, and SLAs) to guide decision-making during operational incidents.

4. Integrate Theoretical Knowledge with Practical Application:
  - Leverage external resources (such as AWS whitepapers, Google SRE documentation, and Red Hat guidelines)
  to deepen understanding of system reliability best practices.
  - Participate in interactive discussion posts and collaborative problem-solving exercises to reinforce learning.

5. Cultivate Analytical and Troubleshooting Abilities:
  - Apply systematic troubleshooting techniques to diagnose and resolve system issues.
  - Reflect on incident case studies and simulated exercises to improve proactive prevention strategies.

These learning objectives are designed to ensure that participants not only grasp the
theoretical underpinnings of system reliability and high availability but also build
the practical skills needed for effective incident management and system optimization
in a professional Linux environment.

## Relevance & Context
Ensuring Mission-Critical Uptime
  - Minimizing downtime is critical, and high availability strategies help ensure 
  continuous service—even in the face of hardware or software failures.

Optimized Incident Management
  - A well-practiced incident triage process enables administrators to quickly diagnose issues,
  reduce system downtime, and mitigate potential service interruptions.

Designing Resilient Architectures
  - For a Red Hat Systems Administrator, understanding how to build redundancy 
  (using techniques like Active-Active or Active-Standby clustering) and eliminate 
  Single Points of Failure (SPOFs) is key to creating robust systems.

Data-Driven Decision Making
  - Leveraging metrics such as KPIs, SLIs, SLOs, and SLAs allows administrators to set
  measurable goals, monitor performance, and make informed decisions about system improvements.

Integration with Enterprise Tools
  - Red Hat environments often utilize specific tools 
  (such as Pacemaker and Corosync for clustering, and Ansible for configuration management) 
  that align with the concepts taught in this unit. Mastery of these principles helps 
  engineers integrate and optimize these tools effectively within their infrastructure.

## Prerequisites

Before engaging with this unit, readers should have a foundational understanding of:

  - **Basic Networking Concepts:** Familiarity with the principles of networking 
  (such as IP addressing, DNS, and basic network troubleshooting) is crucial because many 
  Linux administration tasks involve network configuration and monitoring.

  - **Text Editing and Scripting Basics:** An introductory exposure to editing text
  (using simple editors) and the idea of writing or running small scripts helps 
  prepare learners for more complex shell operations.

  - **Version Control (Git):** Since the learning material and collaborative discussions use GitHub, 
  understanding Git and markdown is beneficial.

  - **Problem-Solving:** A general troubleshooting mindset, including the ability
  to search documentation, diagnose issues systematically, and apply corrective measures.

## Key terms and Definitions

Resilience Engineering
Fault Tolerance
Proactive Monitoring
Observability
Incident Response
Root Cause Analysis (RCA)
Disaster Recovery (DR)
Error Budgeting
Capacity Planning
Load Balancing Service Continuity
Infrastructure as Code (IaC)
Configuration Management
Preventive Maintenance
DevOps Culture

## Lecture Notes

Disks, Filesystems, and Mountpoints
  - lsblk
  - blkid
  - tune2fs
  - dumpe2fs
  - mount
  - lsof

How operational workflows happen
  - Tools we use
  - Who our customers are
  - Key performance indicators


Where does our work come from?
  - We track and find issues that arise from our normal monitoring activities
    - we crawl around on systems
    - How proactive we are is up to us
      - some nights we may have nothing to do. some days we have a lot
  - We get trouble tickets from users
    - Goal is to finish quick
    - Keeping the system up is a KPI. Another KPI is keeping users up and running.
    - We have to triage incidents to decide what to work on first
      - many times the squeaky wheel gets the oil

Take ownership of problems.
Warm handoffs
  - If i think something is high priority, don't just put in a queue, communicate.
Asking for help is vital.

Admin tasks example fields
  - number
  - summary
  - image repository
  - risk score
  - severity
  - source

Kanban board (Kanban Methodology)

### Working with Management

Morning meetings should take no more than 15 minutes and focus on 3 topics.
  - What did you accomplish yesterday?
  - What are you working on today?
  - What blockers do you have for any tasks?

These 4 points form the basis for "Agile" methodology
  1. Individuals and interactions over processes and tools
  2. Working software over comrehensive documentation
  3. Customer collaboration over contract negoation
  4. Responding to change over following a plan


### LVM - Visual

![LVM](lvm.png)


