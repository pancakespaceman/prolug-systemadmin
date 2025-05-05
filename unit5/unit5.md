# Unit 5 - Managing Users and Groups

## Overview

This unit focuses on managing user's environments and scanning and enumerating Systems.

	- Become familiar with networking scanning tools
	- Understand the functionality systems files and customized .(dot) files.

## Learning Objectives

1. Become familiar with Networking mapping:
	- Learn how to find your network inventory by using nmap.
	- Grasp the basics of targeted scans by scanning virtual boxes and creating a report.
2. Explore the system files:
	- Understand the structure of the /etc/passwd file by using the cat command.
	- Customize the /etc/skel file to create a default shell environment for the users.

## Prerequisites

- Basic understanding of networking.
- Familiarity with nmap.
- Intermediate understanding of file manipulation commands.
- General idea of bash scripting.

## Key Terms and Definitions

Footprinting
  - Footprinting is the initial phase of ethical hacking, involving the collection
  of information about a target systems or network to identify potential vulnerabilities.
  This can be done throuhg passive methods (e.g. public records) or active methods
  (e.g. network scanning)
  - [https://en.wikipedia.org/wiki/Footprinting]
  - [https://www.techtarget.com/searchsecurity/definition/footprinting]
  - [https://www.tripwire.com/state-of-security/understanding-cybersecurity-footprinting-techniques-and-strategies]

Scanning
  - Scanning involves probing a network or system to discover open ports, services,
  and potential vulnerabilities. It's typically performed after footprinting
  to gather more detailed information about the target.
  - [https://csrc.nist.gov/glossary/term/vulnerability_scanning]

Enumeration
  - Enumeration is the process of extracting detailed information about a system,
  such as user accounts, network resources, and shares. This step follows scanning
  and provides data that can be used for further exploitation.
  - [https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/enumeration-ethical-hacking/]
  - [https://www.subrosacyber.com/en/blog/what-does-enumeration-mean-in-cyber-security]

System Hacking
  - System hacking refers to the techniques used to gain unauthroized access to
  computers and networks. This includes activities like password cracking, privilege
  escalation, and installing backdoors.
  - [https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/system-hacking-definition-types-processes/]
  - [https://study.com/academy/lesson/what-is-system-hacking-definition-types-process.html]

Escalation of Privilege
  - Privilege escalation is the act of exploiting a bug or design flaw in a system
  to gain elevated access to resources that are normally protected. It can be
  vertical (gaining higher privileges) or horizontal (access peer-level privileges).
  - [https://www.crowdstrike.com/en-us/cybersecurity-101/cyberattacks/privilege-escalation/]
  - [https://en.wikipedia.org/wiki/Privilege_escalation]

Rule of Least Privilege
  - The principle of least privilege dictates that users and programs should
  operate using the minimum privileges necessary to complete a task. This reduces
  the risk of accidental or malicious system compromise.
  - [https://csrc.nist.gov/glossary/term/least_privilege]
  - [https://www.cyberark.com/what-is/least-privilege/]

Covering Tracks
  - Covering tracks involves techniques used by attackers to hide their unauthorized
  activites on a system. This can include deleting logs, hiding files, or modifying
  timestamps to avoid detection.
  - [https://www.infosecinstitute.com/resources/hacking/covering-tracks-of-attacks/]
  - [https://www.globalknowledge.com/us-en/resources/resource-library/articles/the-5-phases-of-hacking-covering-your-tracks/]

Planting Backdoors
  - Plating a backdoor refers to the practice of installing a hidden method for
  bypassing normal authentication to gain access to a system. Backdoors can be used
  for future unauthorized access without detection.
  - [https://www.techtarget.com/searchsecurity/definition/back-door]
  - [https://en.wikipedia.org/wiki/Backdoor_(computing)]

## Lecture Notes

### This Week

Users, groups, and security

[attack.mitre.org]

### Scanning and enumerating systems

Network scan (sweep range)
Target scan (sweep device)
writing a report about the data
	- what has changed
	- what's there
	- what would you want to give out in a report
		- 2 versions, short and long
system names and IPs
system open ports and services
system type, if it can be determined


https://attack.mitre.org/


How do people compromise systems?


