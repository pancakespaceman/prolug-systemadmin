# Unit 7 - Package Management & Patching

## Overview

Managing software on a Linux system involves two essential practices: package
management and patching. Together, they ensure that systems remain functional,
up-to-date, and secure by handling software installation, updates, and vunerability
remediation.
	- **Package Management:** The system used to install, upgrade, configure, and
	remove software packages while automatically resolving dependencies and maintaining
	system consistency.
	- **Patching:** the process of applying updates to software packages or the
	kernel to fix bugs, close security vulnerabilities, or improve performance.

## Learning Objectives

1. Become familiar with package management
	- In this unit you will see what comprises a RPM, YUM, and RPM package and
	how professional administrators carefgully choose what is installed on a system.
2. Become familiar with Patching:
	- Learn about general patching cycles
	- Understand why it matters to inspect packages and associated dependancies.
	- Get hands on with inspecting packages.

## Relevance & Context

The skills taught in this unit are indespecable for several reasons:

	- **Efficient System Management:**
	The RHEL environment is typically managed via the command line. Proficiency in
	the CLI, along with an in-depth understanding of the file system, is crucial
	for daily tasks like system configuration, package management (using tools such
	as YUM or DNF), and remote troubleshooting
	- **Security and Stability:**
	Editing configuration files, managing system services, and monitoring logs are
	all critical tasks that ensure the secure and stable operation of RHEL systems.
	A robust understanding of these basics is necessary to mitigate risks and ensure
	compliance with enterprise security standards.
	- **Professional Certification & Career Growth:**
	For those pursuing certifications like the Red Hat Certified System Administrator
	(RHCSA) or Red Hat Certified Engineer (RHCE), these foundational skills are not
	only testable requirements but also a stepping stone for more advanced topics 
	such as automation (using Ansible), container management (with Podman or
	OpenShift), and performance tuning.
	- **Operational Excellence:**
	In enterprise seetings where uptime as rapid incident response are paramout,
	having a solid grasp of these fundamentals enables administrators to quickly
	diagnose issues, apply fixes, and optimize system performance--thereby directly
	impacting business continuity and service quality.

## Prerequisites

- Lab Access
- Basic understanding of install or update software using DNF.
- Familiarity with terminal commands and accessing man pages.
- Basic understanding of editing config files using vi.

## Key Terms and Definitions

Yum

DNF

Repo

GPG Key

Software dependency

Software version

Semantic Version

## Lecture Notes

Package management
	- How do we update?
	- When do we update?
	- removing old/unused software?
	- here software packages are stored?
	- when were they last updated?
	- what files are brought in with the package from a package manager?


### What are repos?

Places that your system trusts to allow the package managers to install software
from.

	- can be local
	- can be remote
	- you can enable or disable them as you wish
	- you can force GPG verification of all softrware found in the repository


### What is semantic versioning

semver.org

Given a version number MAJOR>MINOR.PATCH, increment the

MAJOR version when you make incompativle API changes
MINOR version when you add functionality in a backward compatible manner
PATCH vesion when you make backward compatible bug fixes

Additional labels for pre-release and build metadata are available as
extensions to the MAJOR.MINOR.Patch format.

kernel-core-5.14.0-503.23.1.e19_5x86_64



`rpm -qi kernel-core`
name, version, release, architecture, install date, group, size, license, 
signature, source RPM, build date, built host, packager, vendor, url, summary,
description

this can tell us everything that is relevant.

Server software update cycle

+-------------------+      Sync/Download       +--------------------------------+
 Analyze/Assess Risk
|  Vendor Patches   | ---------------------> |   Central Patch Management Sys   | ----
--------------------> [ Security/Ops Team ]
| (Red Hat, Oracle, |      (Scheduled)       | (e.g., Satellite, Ansible, etc.) |
 (Review CVEs, Impact)
|   Microsoft, etc.)|                        |    [ Internal Repo/Content ]     |
+-------------------+                        +----------------+-----------------+
                                                              | Select & Approve Patch
es
                                                              V
+--------------------------------+ <--------------------------+
|   Central Patch Management Sys   |
| (Create Patch Baselines/Jobs)  |
+----------------+-----------------+
                 |
 Deploy Patch Baseline/Job (Scheduled)
                 |
                 V
+--------------------------------+      +--------------------------------------+
|       DEV Environment          | ---> |              Testing &                |
| (Initial Deployment/Smoke Test)|      |            Validation (DEV)          |
+----------------+----------------+      +-------------------+------------------+
                 | OK?                                        | Issues Found? -> (Fix/
Troubleshoot Cycle)
                 V Yes                                        | No (Proceed)
+--------------------------------+                            V
|   Central Patch Management Sys   |---------------------------+
|  (Promote Baseline/Schedule)   |
+----------------+-----------------+
                 |
 Deploy Patch Baseline/Job (Scheduled)
                 |
                 V
+--------------------------------+      +--------------------------------------+
|     TEST / QA Environment      | ---> |       Testing & Validation (QA)      |
|  (Functional/Integration Test) |      |   (App Teams, QA Personnel)          |
+----------------+----------------+      +-------------------+------------------+
                 | OK?                                        | Issues Found? -> (Fix/
Troubleshoot Cycle)
                 V Yes                                        | No (Proceed)
+--------------------------------+                            V
|   Central Patch Management Sys   |---------------------------+
|  (Promote Baseline/Schedule)   |
+----------------+-----------------+
                 |
 Deploy Patch Baseline/Job (Scheduled)
                 |
                 V
+--------------------------------+      +--------------------------------------+
+-----------------+
|   STAGING / Pre-Prod Env.    | ---> |    Testing & Validation (Staging)    | ---> |
Change Management |
| (User Acceptance Test - UAT)   |      |   (App Owners, Final Checks)         |
|  (CAB Approval) |
+----------------+----------------+      +-------------------+------------------+
 +--------+--------+
                 | OK & Approved?                             | Issues Found? -> (Fix/
Troubleshoot Cycle) |
                 V Yes                                        | No (Proceed with Appro
val)             |
+--------------------------------+                            V
               |
|   Central Patch Management Sys   |---------------------------+----------------------
-----------------+
| (Schedule PRODUCTION Deployment)|
+----------------+-----------------+
                 | Deploy during Maintenance Window
                 V
+--------------------------------+      +--------------------------------------+
+-----------------+
|     PRODUCTION Environment     | ---> |      Verification & Monitoring       | --->
|   Post-Patch    |
|    (Apply Patches Carefully)   |      |    (Ops Team, Monitoring Tools)      |
|   Reporting     |
+----------------+----------------+      +-------------------+------------------+
 +--------+--------+
                 |                                            | Issues Found?
               | Compliance Audit
                 +--------------------------------------------+-----------------------
----------------+
                 |                                            V
                 | OK                                +-----------------+
                 V                                   |   Rollback Plan |
          [ Patch Cycle Complete ]                   |   (If Severe    |
                                                     |     Issues)     |
                                                     +-----------------+





---

+-----------------+      Reads Global Settings       +-----------------------+
|   dnf / yum     | <------------------------------ | /etc/dnf/dnf.conf     |
| (Package Mgr)   |                                 +-----------------------+
+--------|--------+
         | Reads Repo Definitions
         V
+-----------------------------+
|    /etc/yum.repos.d/        | (Directory holding .repo files)
|  +-----------------------+  |
|  |   redhat.repo         |<-+------ [ Managed by Subscription Manager ]
|  |  (Official RHEL Repos)|  |
|  +-----------------------+  |
|  +-----------------------+  |
|  |   epel.repo           |  | (Example: Extra Packages)
|  |  (3rd Party / Custom) |  |
|  +-----------------------+  |
|  +-----------------------+  |
|  |   my-internal.repo    |  | (Example: Local/Internal)
|  |  (3rd Party / Custom) |  |
|  +-----------------------+  |
|           |                 |
|           | Defines:        |
|           +-> baseurl/mirrorlist --> [ Remote Repositories (Servers) ]
|           |                            (HTTP, HTTPS, FTP, File)
|           +-> gpgkey path --------> +-----------------------------+
+-----------------------------+         |     /etc/pki/rpm-gpg/       | (Directory for
 GPG Keys)
                                        |  +-----------------------+  |
                                        |  | RPM-GPG-KEY-redhat-* |  |
                                        |  | RPM-GPG-KEY-epel-* |  |
                                        |  +-----------------------+  |
                                        +-------------^---------------+
                                                      |
                                                      | Used by dnf/yum for
+-----------------+                                   | Package Signature Verification
|   dnf / yum     | ----------------------------------+
| (Package Mgr)   |
+--------|--------+
         | Stores Metadata / Downloads Packages
         V
+-----------------------------+
|      /var/cache/dnf/        | (Cache Directory)
|  +-----------------------+  |
|  | repo_id_cache_data/   |  | (Subdirs for each enabled repo)
|  +-----------------------+  |
+-----------------------------+

---







