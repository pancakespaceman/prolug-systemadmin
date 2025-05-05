# Unit 5 Worksheet - Managing Users and Groups

## Resources

[https://owasp.org/www-project-top-ten/]
[https://attack.mitre.org/]
[https://www.cobalt.io/blog/defending-against-23-common-attack-vectors]

## Discussion Posts

### 1

Review the page: https://attack.mitre.org/

1. What terms and concepts are new to you?

```
Many are new to me. Just to pick out a few:

Drive-by Compromise, ESXi Administration Command, BITS Jobs, Abuse Elevation Control Mechanism,
Execution Guardrails, Adversary-in-the-Middle, Internal Spearphishing
```

2. Why, as a system administrator and not directly in security, do you think it’s 
so important to understand how your systems can be attacked? Isn’t it someone 
else’s problem to think about that?

```
Let's say you've started to learn how to box. You spend hours every day, trying to
become the best boxer. You learn all the different ways someone can throw a punch
and how to counter it. You feel prepared for anything.
You get into a match with someone and you prepare yourself for whatever punch
is thrown your way. Then your opponent throws a kick at your knee. It hits you
because it never even occured to you to defend against kicks.

As a system admin its our job to maintain and secure systems. If we don't know
how our systems can be attacked, how can we defend them?
```

3. What impact to the organization is data exfiltration? Even if you’re not a data
owner or data custodian, why is it so important to understand the data on your systems?

```
Data is the backbone of any organization. For the protection of the organization,
its employees, and customers, its vital this data not be exfiltrated. It if is,
it could mean major harm to all parties involved.
```

### 2

Find a blog or article on the web that discusses the user environment in Linux. 
You may want to search for .bashrc or (dot) environment files in Linux.

1. What types of customizations might you setup for your environment? Why?

```
We can just look to my dotfiles repo for an example, lol
https://github.com/midsized-sedan/dotfiles

- .bashrc file for aliases and PATH configuration
- terminal emulator configurations
- application specific configurations
	- text editors like neovim
	- terminal multiplexors like tmux
```

2. What problems can you anticipate around helping users with their dot files?

```
Configuration conflicts: From application version support to differences in 
distros, a user's personal configuration can conflict with what the system is
setup for.
dotfile management: how does the user manage their dotfiles? how simple is it
to setup.
User authorization: depending on what the user has in their dotfiles, there may
be conflicts between the user's expectd access level and what they actually have
access to in a system
```

## Definitions

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

## Digging Deeper

1. Read this page: https://owasp.org/www-project-top-ten/
  - What is the OWASP Top Ten?
  - Why is this important to know as a system administrator?

2. Read this article: https://www.cobalt.io/blog/defending-against-23-common-attack-vectors
  - What is an attack vector?
  - Why might it be a good idea to keep up to date with these?


## Reflection Questions

1. What questions do you still have about this week?

2. How are you going to use what you've learned in your current role?


