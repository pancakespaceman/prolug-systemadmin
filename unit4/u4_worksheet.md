# Unit 4 Worksheet - Operating Running Systems

## Resources / Important Links

(Operations Bridge)[https://cio-wiki.org/wiki/Operations_Bridge]
(Security Incident Cheatsheet)[https://zeltser.com/media/docs/security-incident-survey-cheat-sheet.pdf?msc=Cheat+Sheet+Blog]
(Battle Drills)[https://en.wikipedia.org/wiki/Battle_drill]

## Discussion Posts

### 1

Read this article https://cio-wiki.org/wiki/Operations_Bridge

1. What terms and concepts are new to you?

```md
Operations bridge, Root cause analysis, incident management, automation and orchestration,
service availability and performance optimization, proactive monitoring and issue
resolution. Many of the concepts mentioned in the article I understand conceptually,
but would not be able to implement them.
```

2. Which pro seems the most important to you? Why?

```
I think centralized management and streamlined operations is the most important.

To draw a parallel, my last company was working on a wiki of information to become
a Single Source of Truth, a centralized place where all standard operating 
procedures, documentation, and other information was held. If anyone had
any questions, they could look there. If someone had information that differed
from the wiki, then that person had wrong information.

In the same way that such a wiki is beneficial for a team to operate, having
such a setup where all operations can be done from one centralized location
means ease of use and organization across the team and even across many teams.
There is only one place you need to look and worry about, which removes
a ton of the mental overhead burden of remembering multiple locations.
```

3. Which con seems the most costly, or difficult to overcome to you? Why?

```
I think "Required integration with multiple systems and data sources" is the most
costly and/or difficult. However, that depends on at what point in the lifecycle
of your infrastructure are you implementing an Operations Bridge.
If you're implementing the operations bridge at the very beginning of building
out infrastructure and systems, then its a lot more manageable. However, if you're
trying to implement an Operations Bridge into existing systems/infrastructure, it
can be overwhelming complex depending on the scale of what exists already.
```


### 2

Scenario

```
Your team has no documentation around how to check out a server during an incident. 
Write out a procedure of what you think an operations person should be doing on 
the system they suspect is not working properly.
```

This may help  to get you started https://zeltser.com/media/docs/security-incident-survey-cheat-sheet.pdf?msc=Cheat+Sheet+Blog
You may use AI for this, but let us know if you did.

```md
I'm not sure how procedures are actually written and formatted.
I would love any feedback anyone has, or at least I'll read
all of the other procedures others write and see how to improve mine.

Procedure:
0. DOCUMENT each step, what was done, what was found
1. DO NOT install/uninstall programs or run any programs that touch many files.
2. Check event logs
	- are there any unusual events?
	- /var/log, /var/adm, /var/spool
3. Check network configurations and details
	- Are there any anomalous ports or sessions?
	- netstat -nap
	- arp -an
4. Check users
	- look for accounts that do not belong or should have been disabled
	- more /etc/passswd
5. Check processes
	- any processes that are running that shouldn't?
	- ps -ef
6. Check scheduled jobs
	- are there any jobs scheduled that shouldn't be
	- more /etc/crontab
	- ls /etc/cron.*
	- ls /var/at/jobs
7. Check DNS and hosts
	- are there entries that don't belong
	- more /etc/resolv.conf
	- more /etc/hosts
8. Contact your manager
	- don't use email or instant message
	- explain your suspicions and findings
```


## Definitions

Detection:
	- Detection involves identifying potential security incidents through monitoring
	systems, analyzing logs, and utilizing intrusion detection tools. It is the first
	step in recognizing that an incident may be ocuring.
	- [https://www.techtarget.com/searchsecurity/definition/incident-response]

Response:
	- Response refers tot he action taken to address and manage the aftermath of a 
	dedicated security incident. This includes containment, eradication, and
	recovery efforts to minimize impact and restore normal operations.
	- [https://www.solarwinds.com/resources/it-glossary/incident-response]

Mitigation:
	- Mitigation involves steps taken to limit the severity and impact of a security
	incident. This can include isolation affected systems, applying patches, or
	implementing temporary fixes to prevent further damage.
	- [https://www.techtarget.com/searchsecurity/definition/incident-response]

Reporting:
	- Reporting is the process of documenting and communicating details about a
	security incident to relavant stakeholders, inluding internal teams and external
	entities, as required by policies or regulations.
	- [https://www.techtarget.com/searchsecurity/definition/incident-response]

Recovery:
	- Recovery entails restoring and validating system functionality and data
	integrity after an incident. This phase ensures that systems are securly returned
	to normal operations.
	- [https://en.wikipedia.org/wiki/Computer_security_incident_management]

Remediation:
	- Remediation involves identifying and addressing the root cause of a security
	incident to prevent recurrence. This may include correcting vulnerabilities
	and improving security measures.
	- [https://www.techtarget.com/searchsecurity/definition/incident-response]

Lessons Learned:
	- Lessons Learned is a post-incident analysis phase where organizations assess
	what occurred, evaluate the effectiveness of the response, and indetify
	improvements to prevent future incidents.
	- [https://ir.cnd.ca.gov/6.LessonsLearned/]

After action review:
	- An After Action Review is a structure debriefing process used to analyze what
	happened during an incident, why it happened, and how it can be done better.
	It focuses on performance improvement without assigning blame.
	- [https://ir.cnd.ca.gov/6.LessonsLearned/]

Operations Bridge:
	- A centralized monitoring and management platform that provides real-time visibility
	and control over an organization's IT infrastructure and services.
	- Designed to streamline IT operations, improve service ability, and 
	enhance incident reponse and resolution.

## Digging Deeper

1. Read about battle drills here https://en.wikipedia.org/wiki/Battle_drill

2. Why might it be important to practice incident handling before an incident occurs?

3. Why might it be important to understand your tools before an incident occurs?

## Reflection Questions

1. What questions do you still have about this week?

2. How much better has your note taken gotten since you started? What do you still
need to work on? Have you started using a different tool? Have you taken more notes?