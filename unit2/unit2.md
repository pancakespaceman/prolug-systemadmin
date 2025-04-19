# Unit 2 Lecture Notes


Getting history and logs from a terminal sometimes don't work properly.
Use something like tmux to help make sure you get your history.


Note Taking
  - In your terminal
  - outside a terminal
  - drawing

System permissions:
  - ACLs
  - SELINUX

Administration troubleshooting

What is the difference between engineering and admin troubleshooting?

  - In admin, we can assume it was working at some point
  - In admin, we likely have some type of documentation to go from the build or runtime
  (if we do not find build or runtime docs, we are responsible to build runtime docs.
  someimtes called operational docs)
  - In admin, we can compare one system to another working system for differences.


During the lab we'll learn how to perform many admin tasks

- Powering on and off servers
- Maintaining inventory in a system
  - adding, changing, and removing inventory
- Installing and updating software
  - configuring repositories
  - configuring software to work within the cluster
- Responding to incidents
  - System level failures
  - Security incidents


Networking tools of the trade

- ip addr, ip -br addr, ip route, ip neigh
- ping
- arp
- nslookup, host, dig
- ss or netstat
- nc or telnet


Everything we do in the systems world is either checking or setting values in a terminal.


USE Method

For every resource check
- Utilization
  - how much of the the system am I using?
- Saturation
  - queue length, queued time
- Errors
  - errors reported in logs, they should be




https://killercoda.com/fishermanguybro


## Learning Objectives

Understand and Configure SELinux:

1. Grasp the core concepts of SELinux, including security contexts, labels, and its role 
in enforcing mandatory access control.
  - Learn how to configure and troubleshoot SELinux settings to ensure system security and compliance.
  - Master Access Control Lists (ACLs):

2. Recognize the limitations of traditional Unix permissions and how ACLs provide granular 
control over file and directory access.
  - Develop skills in applying and managing ACLs in a complex Linux environment.
  - Develop Effective Troubleshooting Methodologies:

3. Acquire techniques to diagnose and resolve system access issues, particularly 
those arising from SELinux policies and ACL misconfigurations.
  - Apply structured troubleshooting strategies to ensure minimal downtime and maintain high availability.
  - Integrate Theoretical Knowledge with Practical Application:

4. Engage with interactive exercises, discussion prompts, and real-world scenarios to reinforce learning.
  - Utilize external resources, such as technical documentation and instructional videos, to supplement hands-on practice.
  - Enhance Collaborative Problem-Solving Skills:

5. Participate in peer discussions and reflective exercises to compare different approaches 
to system administration challenges.
  - Learn to articulate and document troubleshooting processes and system configurations for continuous improvement.
  - Build a Foundation for Advanced Security Practices:

6. Understand how SELinux and ACLs fit into the broader context of system security and 
operational stability.
  - Prepare for more advanced topics by reinforcing the fundamental skills needed to manage and
  secure Red Hat Enterprise Linux environments.




