# Notes from Crisis Management Whitepaper

(whitepaper)[https://google.github.io/building-secure-and-reliable-systems/raw/ch17.html]


## Opening

We want to keep our systems up, even when malicious actors are attacking. 
The reliability of your systems is a measure of how well your organization can 
withstand a security crisis, and reliability directly impacts the happiness of your
users.

This Chapter:
  - How to recognize a crisis
  - Detailed plan of how to take command and maintain control of an incident
  - communication related pitfalls
  - examples and templates
  - sample crisis scenario

Incidient Management at Google (IMAG) framework


"There are only two types of companies: those that know they've been compromised,
and those that don't know."
Incident Response (IR)
Incident Notification is a core feature of security domain, same with tech advances
such as cloud computer, "bring your own device", and IoT.

## Is it a Crisis or not?

Not every incident is a crisis. Relatively few incidents should turn into
crises if your org is in good shape.

Triage: using the knowledge and information available to make educated and informed
assumptions about the severity and potential consequences of the incident.

### Triaging the Incident

Gather basic facts to help decide whether the escalation is one of the following:
  - An error (i.e., a false positve)
  - An easily correctable problem (an opportunistic compromise, perhaps)
  - A complex and potentially damaging problem (such as a target compromise)

Should be able to triage predictable problems, bugs, and other straightforward issues
using predetermined processes. Larger issues will likely require an organized and
managed response.

Every team should have preplanned criteria for what constitutes an incident. 
Response to an incident will depend on the type of environment, the state of
the organization's preventative controls, and sophistication of its response 
program.
3 examples of orgs responding to a ransomware attack:
  - Org 1
    - mature security process, layered defenses including a restriction that permits
    only cryptographically signed and approved software to execute.
    - highly unlikely that well-known ransomware can infect a machine or spread
    through the network
    - if it does, detection system raises an alert and is investigated
    - single engineer can handle the issue due to mature processes and layered
    defenses
    - does not require a crisis-style incident response
  - Org 2
    - sales department that host customer demos in a cloud environment
    - users tend to make security config mistakes that result in system
    compromises
    - security team implements a mechanism to automatically wipe and replace
    compromised cloud test instances
    - ransomware would not require much forensics or incident response attention
    - doesn't prevent attack, but has automated mitigation tools
  - Org 3
    - fewer layered defenses and limited visibility into whether systems are 
    compromised
    - at greater risk. may not be able to respond quickly
    - large number of business critical systems may be affected if the worm spreads
    - required significant technical resources to rebuild the compromised networks
    and systems
    - the worm is a serious risk

Org 1 may need to simply initiate a playbook-driven respose, Org 3 may face
a crisis that requires coordinated incident management.

Questions to assess if escalation requires playbook-driven approach or 
crisis management approach.
  - What data do you store that might be accessible to someone on that system?
  What is the value or criticality of that data?
  - What trust relationships does the potentially compromised system have with
  other systems?
  - Are there compensating controls that an attacker would also have to penetrate
  (and that seem intact) in order to take advantage of their foothold?
  - Does the attack seem to be commodity opportunistic malware (e.g. Adware),
  or does it appear more advanced or targeted (e.g. a phishing campaign seemingly
  creafted with your organization in mind)?

### Compromises Versus Bugs

Bugs are inevitable, and you can plan for them. Good defensive practices
remove or limit the potential negative consequences of vulnerabilitys before they
begin.

At Google, vulnerabilities that carry extreme risk as incidents.
Coordinated vulnerability disclosures (CVDs)

**Coordinated Vulnerability Disclosure**
There are many interpretations of what CVD means. The emergent (ISO standard 29147:2018)[https://www.iso.org/standard/72311.html]
provides some guidance. At Google, we generally define CVD as a process in which
a team must maintain a careful balance between the amount of time it may take the
vendor to issue security patches, the needs and wishes of the person who finds
or reports the bug, and the needs of the user base and customers.


## Taking Command of Your Incident

### The First Step: Don't Panic!

Don't rush. Panic doesn't help and not planning can make things worse.

Difference between beginning a crisis management response for a security incident
and for a reliability incident such as an outage. If an outage, onsite SRE prepares
to step into action to find and fix errors to restore the system. System is not
conscious of its actions and won't resist being fixed.

An attacker may pay attention to actions taken by the target org and work against
responders. A full investigation must be done first to mitigate.
SRE doesn't carry the same risk typically, so they can just fix the system first
then document what happened.

First task of security response is control your emotions. Panic can make things worse.
Additional planning up front most always ensures a better result than immediate
action.

### Beginning Your Response

Incident Management at Google, a standard process. based on (Incident Command System)[https://en.wikipedia.org/wiki/Incident_Command_System]

First step is to take _command_ with a declaration:
"Our team is declaring an incident involving X, and I am the incident commander (IC)."
Being explicit is important to avoid misunderstandings.

Second step is to keep _control_ of the incident.
IC directs the response and ensures people are always moving forward. Excellent
communications is a must.

### Establishing Your Incident Team

Under IMAG, either the person declaring the incident becomes the IC or selects
someone to be the IC.

IC to assess what actions need to be taken immediately and who can fill those roles.
staff should know the potentially affected systems well.

If a incident needs a larger team, a few leaders can be put in place to handle
certain aspects of the investigation.

  - Operations Lead (OL): tactical counterpart to the IC. OL focuses on meeting
  the goals of the IC and determining how to do so.
  - Management Liaison: someone to make tough calls on a spot. Who from your org
  can decide to shut down a revenue-generating service if necessary? Who can decide
  to send staff home or revoke the redentials of other engineers?
  - Legal Lead: incident may reaise legal questions. Do your employees have an
  enhanced expectation of privacy? For example, if you think someone downloaded
  malware via their web browser, do you need extra permissions to examin their
  browser history? What if you think they downloaded malware via their personal
  browser profile?
  - Communications lead: May need to communicate with customers, regulators, etc.

### Operational Security

Operational Security (OpSec) refers to keeping your response activity secret. There
will likely be info that needs to be kept secret, at least for a limited amount
of time.

IC is responsible for ensuring that rules around confidentiality are set,
communicated, and followed.

IC should document specific guidance for each incident response team member.
Each team member shoudl review and acknowledge before starting work on the incident.

OpSec plan address how the response can proceed withour further exposing
response activity and the organization.
  - Attacker compromises an employee's account and is trying to steal other passwords
  from memory on a server. If a sys admin logged into the affected machine with
  admin credentials that would be great for them.

Cluing in an attacker that you've discovered their attack is really bad.
  - may go quiet
  - may destroy as much of org as they can if they finished

Commong OpSec mistakes:
  - Communicating about or documenting the incident in a medium (such as email)
  that enables the attacker to monitor response activities.
  - Logging into compromised servers. This exposes potentially useful authentication
  credentials to the attacker
  - Connecting to and interacting with the attcker's "command and control" servers.
  For example, don't try to access an attacker's malware by downloading it from a
  machine you're using to perform your investigation. Your actions will stand out in
  the attacker's logs as unusual and could warn them of your investigation.
  Also, don't perform port scanning or domain lookups for the attacker's machines
  (common mistake made by novice responders).
  - Locking accounts or changing passwords of affected users before your
  investigationis complete.
  - Taking systems offline before you understand the full scope of the attack.
  - Allowing your analysis workstations to be accessed with the same credentials
  an attacker may have stolen.

Good Practices for OpSec response:
  - Conduct meetings and discussions in person where possible. If you need to use
  chat or email, use new machines and infrastructure. For exmaple, an org facing
  a compromise of unknown extect might build a new temporary could-based environment
  and deploy machines that differ from its regular fleet (e.g. chromebooks) for
  responders to communicate. Ideally, this tactic provides a clean environment
  to chat, document, and communicate outside of the attacker's view.
  - Whereever possible, ensure that your machines have remote agents or key-based
  access methods configured. This allows you to collect evidence without revealing
  login secrets.
  - Be specific and explicit about confidentiality when asking people to help.
  They may not know particular information is supposed to be kept confidential
  unless you tell them.
  - For each step of your investigation, consider the conclusions a shrewd attacker
  may draw from your actions. For example, an attacker who compromises a Windows
  server may notice a sudden rush of group policy tightening and conclude that
  they've been discovered.

Write out domains and other network-based indicators in ways that your
tools will not match and autocomplete.

### Trading Good OpSec for the Greater Good

If faced with an imminent and clearly identifiable risk, this is an exception
to the general advice of keeping your incident response a secret.
  - if you suspect a compromise to a system so critical that vital data, systems,
  or even lives may be at risk, extreme measures may be justified.
  - say with a vulnerability or bug, if its so easily exploited or widely known,
  sometimes turning off or disabling the system might be the best way to protect it.

Typically executives make such calls, not the ICs. IC is the resident expert in the room.

Its about making, not the _right call_, but the _best possible call_.

