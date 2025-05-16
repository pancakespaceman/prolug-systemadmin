# Unit 7 Worksheet - Package Management & Patching

## Discussion Posts

### 1

1. Why is software versioning so important to software security?

```
Software versioning keeps track of the state of a given piece of software.
The date of release, the code that is included in the program.
This means that it's also useful to track vulnerabilities. As these vulnerabilities
become known, we can ensure the safety of our systems by using software that
are on approved versions.
```

2. Can you find 3 reasons, from the internet, AI, or your peers?

```
Here are some reasons according to this article: https://www.techrepublic.com/article/versioning-benefits/

Instantly identifyable lastest and greatest:
	- Versioning makes it easy to identify software with the most up-to-date features. 
	
Quicker Debugging: 
	- Being able to pinpoint a specific version that introduced a bug allows for
	less time to be spent tracking down the source of a problem
	
Easier Deployments:
	- Once a snapshot of a codebase has been labled, you know for certain what
	will go into that release.
```

### 2 

Scenario:

You are new to a Linux team. A ticket has come in from an application team and has
already been escalated to your manager.

They want software installed on one of their servers but you cannot find any
documentation. Your security team is out to lunch and not responding.

You remember from some early documentation that you read that all the software in
the internal repos you currently have are approved for deployment on servers.
You want to also verify by checking other servers that this software exists.

This is an urgent task and your manager is hovering.


1. How can you check all the repos on your system to see which are active?

```
With the command `dnf repolist` or by checking the `/etc/yum.repos.d/` directory.
```

2. How would you check another server to see if the software was installed there?

```
I would use the `rpm -qa` command, pipe into grep with the name of the software.
```

3. If you find the software, how might you figure out when it was installed? (Time/Date)

```
I would use the `rpm -qi PACKAGE_NAME` command to find out when it was installed. 
```

### 3

Scenario:

Looking at the concept of group install from DNF or YUM, why do you think an
administrator may never want to use that in a running system? Why might an engineer
want to or not want to use that? This is a thought exercise, so it's not a "right
or wrong" answer it's for you to think about.

1. What is the concept of software bloat, and how do you think it relates?

```
In terms of a linux system, software bloat can mean unnecessarily install packages,
services that are running that arn't being used, large dependancy trees, and more.
If we're not careful, its very easy to install programs without considering how many
packages it depends on, including the versions of those dependancies.
Our system could end up with hundreds or more of varying packages, making it 
harder to properly secure and keep performant.
```

2. What is the concept of a security baseline, and how do you think it relates?

```
A security baseline is a set of standardized, minimum security configurations and
settings for a system.
It relates to this situtation in that it provides a guideline for how a system
should look and operate, and also provides a returning point should the system need
to be reconfigured or recovered.
```

3. How do you think something like this affects performance baselines?

```
Software bloat can take up system resources like storage for packages or CPU and RAM
for running services. This can interfer with the performance of our systems.
Having baselines of our system performance can be one way to compare what our
system should be performing at versus how it's actually running.
```

### Responses

[x] Question 1 Response
[x] Question 2 Response
[x] Question 3 Response

## Definitions

Yum:
	- YUM (Yellowdog Updater, Modified) is a command-line package manager for
	RPM-based distributions like RHEL and CentOS. It was the default package
	manager before being preplaced by DNF.

DNF:
	- DNF (Dandified YUM) is the modern replacement for YUM in Fedora, RHEL 8+,
	and related distributions. It improves performance, handles dependencies better,
	and has a cleaner codebase.
	- https://dnf.readthedocs.io/en/latest/

Repo:
	- Repo (Software Repository) is a colelction of packages (binaries and metadata)
	that a package manager like YUM or DNF can use to install and update software.

GPG Key:
	- GPG keys are used to sign and verify software packages to ensure they haven't
	been tampered with. Package managers use GPG to check the authenticity of
	packages from a repo.

Software dependency:
	- A software dependancy is a library or package that a program needs to function.
	Dependencies must be present and compatible for the software to work correctly.

Software version:
	- The version of software indicates a particular state of the software at a 
	point in time. It helps track changes, fixes, features, and compatibility.

Semantic Version:
	- Semantic Versioning is a versioning scheme using the format MAJOR.MINOR.PATCH

## Digging Deeper

1. What is semantic versioning? https://semver.org/

## Reflection Questions

1. What questions do you still have about this week?

2. How does security as a system administrator differ from what you expected?




