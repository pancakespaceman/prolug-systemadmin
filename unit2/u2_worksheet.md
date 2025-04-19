# Unit 2 Worksheet - Essential Tools

## Resources / Important Links

(Bash Reference Manual)[https://www.gnu.org/software/bash/manual/bash.html]
(Security Enhanced Linux)[https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/using_selinux/getting-started-with-selinux_using-selinux#getting-started-with-selinux_using-selinux]


## Discussion Posts

### 1

Think about how week 1 went for you.

1. Did you get everything you needed to done?

```
I did everything except I forgot the responses for the discussion board. Going to
make sure I do that this time around.
```

2. Do you need to allocate more time to the course, and if so, how do you plan to do it?

```
I do think I need to allocate more time. While the content itself was mainly review, it
took me longer than I thought to get through it all. It didn't help that I broke my
pc and had to reinstall everything, which took away a few days.
```

3. How well did you take notes during the lecutre? Do you need to improve this?

```
Historically I've always been terrible at taking notes during live lectures. I tend to
find that I pay better attention if I just focus on what is being said. However,
I do plan on doing better with future lectures to write down key points, especially those
not found in the worksheet or lab.
```

### 2 

Read a blog, check a search engine, or ask an AI about SELinux.
What is the significance of contexts? What are the significance of labels?


```
SELinux (Security Enhanced Linux) is a kernel feature that provides more control over 
who can access the system. It uses Mandatory Access Controls, which use security contexts 
to provide the policies the system will follow. These contexts override the traditional 
Discretionary Access Control which is user based. This allows for administrator to mandate
security policies system wide.

Security contexts are the actual rules that describe access. 
They are made up of 3-4 words that describe how and by whom a given resource should be accesses. 
It follows the syntax of user:role:type:level.

Labels are the actual metadata that a given object (e.g. file) has, 
and can be shown with the ls -Z command, or ps -Z for processes. 

Both these terms tend to be used interchangeably, but there is a distinction between how a 
context is made and how it is applied to an object (labeling).
```

Scenario:
```
You follow your company instructions to add a new user to a set of 10 Linux servers.
They cannot access just one of the servers.

When you review the differences in the servers you see that the server they cannot 
access is running SELINUX. On checking other users have no problem getting into the system.

You find nothing in the documentation (typical) about this different system or how these 
users are accessing it.
```

What do you do?
Where do you check?

```
I would first check the user account that I made to make sure everything is setup properly.
Is the account created (id command)? is the shell valid (grep uname /etc/passwd)?
is there any typos in their username or home dir?

can they login locally (su - uname)? If they can login locally, then the issue may
be with how the user is trying to connect and look into that, such as an issue with ssh.

If They can't access locally, then maybe there is an issue with SELinux.
First you could set SELinux to permissive with setenforce 0. If the user account can 
access after that, its for sure a SELinux issue.

You can check the SELinux User Mapping with semanage login -l
If the user isn't listed or has an incorrect mapping, fix it with semanage and try accessing again.

From there, others issues could include:
  - The context labels for an application or file are incorrect.
    - a solution could be to change the default file type for the directory hierachy
  - A boolean that configures a security policy for a service is set incorrectly
    - a solution could be to change a specific boolean value, such as setbool -P httpd_enable_homedirs on
  - A service attempts to access a port to which a security policy does not allow access
    - if the use of the port is valid, add the port to the policy configuration with semanage
  - An update to a package causes an application to behave in a way that breaks an existing security policy
    - audit2allow -w -a to view the reason why access denial occured

Lastly, I would ask for help, assuming I had a team/manager I could reach out to.
```

You may use any online resource to help you answer this. This is not a trick and 
its not a "one answer solution". This is for you to think through.

## Projects Ideas

Topics:
  1. System Stability
  2. System Performance
  3. System Security
  4. System Monitoring
  5. Kubernetes
  6. Programming/Automation

You will research, design, deploy, and document a system that improves your 
administration of Linux systems in some way.


## Definitions

SELinux (Security-Enhanced Linux):
  - A security architecture for linux that allows administrators to have more
  :q
  control over who can access the system.
  - Defines acces controls for the applications, processes, and files on a system
  - SELinux checks permissions with an Access Vector Cache (AVC) upon request
  by a subject (app or process). This is where permissions are cached for subjects and objects
  - If unable to make a decision about access based on cached permissions, it sends
  a request to the security server, which then checks for the security context of the
  app or process and file.
  - Utilizes MAC instead of DAC

Access Control Lists (ACLs):
  - a list that allows for more granual and flexible permissions for access compared
  to the traditional user/group/other model
  - allows setting of read, write, and execute permissions for individual users or groups
  on a per-file or per-directory basis
  - can grant or restrict access to resources without altering the existing group
  structures or ownership settings


High Availability (HA):
  - Refers to systems designed to operate continuously without failure for extended 
  periods of time. 

Service Level Objectives (SLOs):
  - specific measurable characterictics of the service level agreed upon as a means of
  assessing the performance of a service provider.


Uptime:
  - the time the system has been operational without interruptions

Standard input (stdin):
  - The default data stream for input

Standard output (stdout):
  - the default data stream for output

Standard error (stderr):
  - the default data stream for error messages

Standard Streams:
  - preconnected input/output communication channels between a computer program and
  its environment.
  - originally happened via a system console (input w/ keyboard, output w/ monitor)
  - Streams have since been abstracted to allow for other sources of I/O
  - in terms of linux, typically this standard stream is an interactive shell, like bash where
  all input, output, and errors are taken.
  - child processes generally inherit standard streams from its parent process.
  - streams allow for applications to be chained, where output of one program is then
  redirected to the input of another program.

Mandatory Access Control (MAC):
  - Policies that are enforced by the system, not by the user.
  - These poilicies are defined by an administrator, and the system mandates it.
  - SELinux policies override DAC. for example, if a users home directory's DAC settings
  are changes, the MAC settings prevent another user or process from accessing the directory.
  - based on subjects (apps or processes) and objects (files, directories, TCP/UDP ports, 
  shared memory segments, or IO devices)

Discretionary Access Control (DAC):
  - Traditional Linux/Unix access control
  - A user can own a file, a group, or other.
  - Users can change permissions of their own files
  - Root user has full access control

Security contexts (SELinux):
  - the mechanism that MAC uses to classify resources, such as processes and files.
  - comprised of four fields: `user:role:type:level`

Label:
  - actual metadata applied to an object that includes the security context
  - to label a file is to apply it a security context

SELinux operating modes:
  - Enforcing: SELinux policies are enforced.
  - Permissive: SELinux logs violations but doesn't block anything.
  - Disabled: SELinux is turned off.
  - changable at `/etc/selinux/config` or with `setenforce`

Common Tools:
  - `getenforce`, `sestatus`: check status
  - `chcon`: temporarily change a file's context
  - `restorecon`: restore default contexts
  - `semanage`: manage SELinux policies (add rules, port laels, etc.)
  - `audit2allow`: helps turn AVC denials into SELinux policy modules 

Troubleshooting Methodologies:
  - structured approaches used to diagnose and resolve issues within systems


## Digging Deeper

1. How does troubleshooting differ between system administration and system engineering?
To clarify, how might you troubleshoot differently if you know a system was previously running
correct. If you're building a new system out?

(blog on admin vs engineer)[https://www.franklinfitch.com/uk/resources/blog/system-administrator-vs-system-engineer/]

System administrators focus on the maintenance, support, and optimization of existing systems.
  - Tasks include user management, system updates, security monitoring, and ensuring system
  availabitlity
  - Troubleshooting:
    - typically reactive, addressing issues as they arise to restore a system to proper functionality
    - admins focus on immediate, operational issues
    - use monitoring logs, log analysis, patch management systems

System engineers focus on the design, development, and implementation of new systems, or the
enhancement of existing ones.
  - Taks include system architecture planning, integration of new technologies, and ensuring
  scalability.
  - Troubleshooting:
    - typically proactive, anticipating protential design or integration issues during development
    - engineers deal with broader, systemic issues
    - use modeling tools, simulation software, and confuct feasibility studies

Troubleshooting existing systems
  - Approach:
    1. Identify recent changes: if a recent update, config, or environment change cause the problem
    2. analyze logs and alerts: review system logs, error messages, and monitoring alerts
    3. reproduce the issue: replicate the problem in a controlled environment, if possible
    4. Implement fixes: apply patches, revert configurations, adjust settings
    5. Monitor post-fix: monitor the system to ensure stability
  - Considerations
    - Minimize downtime: aim for quick resolutions
    - document changes: keep records of issues and solutions
    - user communication: inform stakeholders about issues and expect resolution times

Troubleshooting new system buils
  - Approach:
    1. Requirement Analysis: ensure system requirements are well-defined and understood
    2. Design Validation: review system designs for feasibility and alignment with requirements
    3. Prototype and test: develop prototypes or modules and test them individually
    4. Integration testing: test for interoperability and performance
    5. User acceptance testing (UAT): engange end-users to validate the system meets their needs
  - Considerations:
    - scalability: design systems that can grow with organizational needs
    - security: incorporate security best practices from the outset
    - documentation: Maintain comprehensive documentation

(Blog - Does your system need upgrade or replacement)[https://transcendentsoftware.com/blog/how-to-tell-if-your-existing-system-needs-an-upgrade-or-replaced-altogether/]


2. Investigate a troubleshooting methodology, by either Google or AI search. Does the
methodology fit for you in an IT sense, why or why not?

I think the CompTIA methodologies fits for me. The main reasons are the emphasis
on data collection to identify the problem first, and also trying to reproduce the issue
when possible. Reproducing an issue really helps to cement what is the root cause of 
an error.

**Troubleshooting methodologies**

Here is a list of different methodologies

**CompTIA Troubleshooting Methodology**
(link)[https://www.comptia.org/blog/troubleshooting-methodology]

1. Identify the problem
  - Gather information (logs, error messages, user reports)
  - Question the user and check for recent changes
  - Indentify symptoms and determine the scope of the issue.
2. Establish a theory of probably cause
  - Start with the obvious (e.g. file permissions, typos)
  - Consider multiple possiblitiies (e.g. ACLs, SELinux denials)
3. Test the theory to determine the cause
  - Confirm your theory by reproducing or resolving the issue
  - if the theory is wrong, go back to step 2
4. Establish a plan of action to resolve the problem and identify potential effects
  - Plan how to resolve the issue with minimal disruption
  - consider backups, system impact, downtime, etc.
5. Implement the solution or escalate as necessary
  - apply the fix and monitor for unintended side effects
6. Verify full system functionality and, if applicable, implement preventative measures
  - ensure everything works properly
  - apply updates, tweak policies, or document the fix to prevent recurrence
7. Document findings, actions, outcomes and lessons learned

**Plan-Do-Check-Act (PDCA) Cycle**
(link)[https://asq.org/quality-resources/pdca-cycle?srsltid=AfmBOooLgLlsgI_Ei4OEhMHruAHyLI5mozapHsik1YBJKVmYOQRJ9hKx]

Used in DevOps, Quality Assurance, and System Engineering

1. Plan - Define the issue and a proposed solution
2. Do - Implement the fix (in a test environment first, is possible)
3. Check - Evaluate the results
4. Act - Apply the solution more broadly or refine it


**NIST Root Cause Analysis**
(link)[https://csrc.nist.gov/glossary/term/root_cause_analysis]


**Google Site Reliability Engineer's "Postmortem" + Incident Response Method**
(books link)[https://sre.google/books/]
(Site Reliability engineering book)[https://sre.google/sre-book/table-of-contents/]
(Workbook)[https://sre.google/workbook/table-of-contents/]

Used by Site Reliability Engineers and DevOps

1. Detect the issue
2. Triage and mitigate impact
3. Resolve the root cause
4. Conduct a postmortem (what went wrong, what was learned)
5. Apply long-term fixes and action items

**Cisco OSI Model**
(Troubleshooting Methods for Cisco IP Networks)[https://www.ciscopress.com/articles/article.asp?p=2273070&seqNum=2#:~:text=The%20top%2Ddown%20troubleshooting%20method,are%20fully%20operational%20as%20well.]
(Pearson Cert)[https://www.pearsonitcertification.com/articles/article.aspx?p=1730891]

Good for network/system issues.

1. Physical - Is the system reachable?
2. Data Link - Is the NIC up? Are packets being sent?
3. Network - Are IP addresses and routes correct?
4. Transport - Are post open and responding?
5. Session/Presentation - is the connection being handle correctly?
6. Application - Is the service or user-level configuration correct?

**Axelos ITIL**
(practice guide)[https://www.axelos.com/resource-hub/practice/readers-manual-itil-4-practice-guide]

1. Categorize the incident
2. Prioritize based on business impact
3. Assign to appropriate team
4. Investigate and diagnose
5. Implement workaround or permanent fix
6. Document root cause and update the knowledge base


**Basic SELinux Troubleshooting in CLI**
(link to rhel)[https://access.redhat.com/articles/2191331?extIdCarryOver=true&sc_cid=701f2000001Css5AAC]



## Reflection Questions

1. What questions do you still have about this week?

My main questions are about how do we setup SELinux on our system.
How do you properly setup security contexs and apply them to the system.
Do all enterprise systems need SELinux? 
When can we use an ACL instead of SELinux? when do we use an ACL AND SELinux?

2. how are you going to use what you've learned in your current role?

I'm not in a role so I'll be using these things to continue my education.


