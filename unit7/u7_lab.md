# Unit 7 Lab - Package Management & Patching

## Resources

https://killercoda.com/learn
https://docs.rockylinux.org/
https://docs.rockylinux.org/guides/package_management/dnf_package_manager/
https://wiki.rockylinux.org/rocky/repo/

## Pre-Lab Warm-Up

For Clarity:

DNF is the 'frontend' aspect of the Rocky package management apparatus and RPM (
RHEL package manager) is the 'backend'. The front end abstracts away and automates the
necessary commands required to install and manipulate packages.

RPM allows for finer control compared to its relted frontend and is intended for
more advanced use cases. The Debian/Ubuntu quivalents are the apt frontend and
dpkg backend.

Investigate the man pages for additional information.

```
cd ~
rpm -qa | more
rpm -qa | wc -l

[root@rocky14 ~]# rpm -qa | wc -l
483


# pick any <name of package> from the above list
rpm -qi {name of package}

[root@rocky14 ~]# rpm -qi firewalld
Name        : firewalld
Version     : 1.3.4
Release     : 9.el9_5
Architecture: noarch
Install Date: Mon Feb 10 18:37:27 2025
Group       : Unspecified
Size        : 2150969
License     : GPLv2+
Signature   : RSA/SHA256, Tue Feb  4 02:33:22 2025, Key ID 702d426d350d275d
Source RPM  : firewalld-1.3.4-9.el9_5.src.rpm
Build Date  : Tue Feb  4 02:32:59 2025
Build Host  : pb-d64c9408-e34d-42fa-b251-d15ddca2e3d9-b-noarch
Packager    : Rocky Linux Build System (Peridot) <releng@rockylinux.org>
Vendor      : Rocky Enterprise Software Foundation
URL         : http://www.firewalld.org
Summary     : A firewall daemon with D-Bus interface providing a dynamic firewall
Description :
firewalld is a firewall service daemon that provides a dynamic customizable
firewall with a D-Bus interface.


rpm -qa | grep -i imagemagick

dnf install imagemagick
# What is the error here? Read it
The error is that package has capital letters, and what we typed does not.

dnf install ImageMagick

# What are some of the dependencies here? Look up the urw-base35
# and see what functionality that adds.
urw-base25 are a collection of font libraries

rpm -qa | grep -i imagemagick

[root@rocky14 ~]# rpm -qa | grep -i imagemagick
ImageMagick-libs-6.9.13.25-1.el9.x86_64
ImageMagick-6.9.13.25-1.el9.x86_64

# Why did this work when the other one didn’t with dnf?
Because the -i flag for grep means search case-insensitive.
```

Math Practice:

Some fun with command line and basic scripting tools. I want you to see some of
the capabilities that are available to you. Your system can do a lot of basic
arithetic for you and this is a very small set of examples.

```
# Check to see if you have bc tool.
rpm -q bc

#Install it if you need to
dnf install bc

for i in 'seq 1 5'; do free | grep -i mem | awk '{print $3}'; done

# Collect the 5 numbers (what do these numbers represent? Use free to find out)
echo "(79 + 79 + 80 + 80 + 45) / 5" | bc

# Your numbers will vary. Is this effective? Is it precise enough?
echo "(79 + 79 + 80 + 80 + 45) / 5" | bc -l
# Is this precise enough for you?

[root@rocky14 ~]# for i in 'seq 1 5'; do free | grep -i mem | awk '{print $3}'; done
3088872
[root@rocky14 ~]# echo "(79 + 79 + 80 + 80 + 45)/5" | bc
72
[root@rocky14 ~]# echo "(79 + 79 + 80 + 80 + 45)/5" | bc -l
72.60000000000000000000

# Read the man to see what the -l option does to bc
man bc

```

It would be astute to point out that I did not have you do bash arithmetic. There
is a major limitation of using bash for that purpose in that it only wants to deal
with integers (whole numbers) and you will struggle to represent statistical data
with precision. There are very useful tools though, and I would highly encourage
you to examine them. https://tldp.org/LDP/abs/html/arithexp.html
 
## Lab

### RPM:

RPM is the RedHat package manager. It is a powerful to see what is installed on your
system and to see what dependencies exist with different software packages. This is
a tool set that was born of the frustration of "dependancy nightmare" where system
admins used to compile from source code only to find they had dependencies. RPM
helps to de-conflict and save huge amounts of time and engineering headaches.

Run through these commands and read man rpm to see what they do.

```
# Read about the capabilities of systemd
rpm -qi systemd

# query the package given
rpm -q systemd

# query all packages on the system (is better used with | more or | grep)
rpm -qa

#for example shows all kernels and kernel tools
rpm -qa | grep -i kernel

# List out files, but only show the configuration files
rpm -qc systemd

# What good information do you see here? Why might it be good to know
# that some piece of software was installed last night, if there is now
# a problem with the system starting last night?
rpm -qi systemd

Tons of useful information, but specific to the question, knowing when software 
is installed can really help to narrow down if the package in question is part of
a problem causing issues.

# Will list all the files in the package. Why might this be useful to you to know?
rpm -ql systemd

If you know certain files to be causing issues, this can help to find out what
files are part of which package.

# List capabilities on which this package depends
rpm -qR systemd

# Probably going to scroll too fast to read. This output is in reverse order.
rpm -q -changelog systemd

# So let’s make it useful with this command
rpm -q -changelog systemd | more

# What are some of the oldest entries?

Sat Nov 16 2024 Release Engineering
	- Set support URL to the wiki
	- Set sbat mail to security@rockylinux.org

# What is the most recent entry?
Fri Dec 02 2022 systemd maintenance team
	- build systemd-boot EFI tools

# Is there a newer version of systemd for you to use?
Yes

# If there isn’t don’t worry about it.
dnf update systemd
```

Use `rpm -qa | more` to find 3 other interesting packages and perform `rpm -qi <package>`
on them to see information about them.

```
sed

[root@rocky14 ~]# rpm -qi sed
Name        : sed
Version     : 4.8
Release     : 9.el9
Architecture: x86_64
Install Date: Sun Nov 19 15:25:20 2023
Group       : Unspecified
Size        : 813607
License     : GPLv3+
Signature   : RSA/SHA256, Mon May 16 07:50:13 2022, Key ID 702d426d350d275d
Source RPM  : sed-4.8-9.el9.src.rpm
Build Date  : Mon May 16 07:46:39 2022
Build Host  : pb-ef2feffd-aa41-475a-9367-0b507424fa4b-b-x86-64
Packager    : Rocky Linux Build System (Peridot) <releng@rockylinux.org>
Vendor      : Rocky Enterprise Software Foundation
URL         : http://sed.sourceforge.net/
Summary     : A GNU stream text editor
Description :
The sed (Stream EDitor) editor is a stream or batch (non-interactive)
editor.  Sed takes text as input, performs an operation or set of
operations on the text and outputs the modified text.  The operations
that sed performs (substitutions, deletions, insertions, etc.) can be
specified in a script file or from the command line.

-----

strace

[root@rocky14 ~]# rpm -qi strace
Name        : strace
Version     : 5.18
Release     : 2.el9
Architecture: x86_64
Install Date: Fri Jun  7 22:44:12 2024
Group       : Unspecified
Size        : 2994672
License     : LGPL-2.1-or-later and GPL-2.0-or-later
Signature   : RSA/SHA256, Wed Sep 28 16:15:53 2022, Key ID 702d426d350d275d
Source RPM  : strace-5.18-2.el9.src.rpm
Build Date  : Wed Sep 28 15:57:16 2022
Build Host  : pb-e7acabfd-ec56-4e2c-8df8-c45ed8423ca0-b-x86-64
Packager    : Rocky Linux Build System (Peridot) <releng@rockylinux.org>
Vendor      : Rocky Enterprise Software Foundation
URL         : https://strace.io
Summary     : Tracks and displays system calls associated with a running process
Description :
The strace program intercepts and records the system calls called and
received by a running process.  Strace can print a record of each
system call, its arguments and its return value.  Strace is useful for
diagnosing problems and debugging, as well as for instructional
purposes.

Install strace if you need a tool to track the system calls made and
received by a process.

---

oniguruma

[root@rocky14 ~]# rpm -qi oniguruma
Name        : oniguruma
Version     : 6.9.6
Release     : 1.el9.6
Architecture: x86_64
Install Date: Mon Feb 10 18:35:43 2025
Group       : Unspecified
Size        : 763442
License     : BSD
Signature   : RSA/SHA256, Mon Nov  4 14:15:03 2024, Key ID 702d426d350d275d
Source RPM  : oniguruma-6.9.6-1.el9.6.src.rpm
Build Date  : Mon Nov  4 14:13:09 2024
Build Host  : pb-119917f2-c1fb-404c-b951-4b31f340ba1c-b-x86-64
Packager    : Rocky Linux Build System (Peridot) <releng@rockylinux.org>
Vendor      : Rocky Enterprise Software Foundation
URL         : https://github.com/kkos/oniguruma/
Summary     : Regular expressions library
Description :
Oniguruma is a regular expressions library.
The characteristics of this library is that different character encoding
for every regular expression object can be specified.
(supported APIs: GNU regex, POSIX and Oniguruma native)

```

### DNF:

DNF is the front end package manager implemented into Rocky and derives its roots
from Yum, a log decrepit version of Linux called Yellow dog. It is originally the
Yellowdow Update Manager. It has a very interesting history surrounding PS3, but
that and other nostalgia can be found here if you're interested:
https://en.wikipedia.org/wiki/Yellow_Dog_Linux


We're going to use DNF to update our system. RHEL and CentOS systems look to repositories
of software for installation and updates. We have a base set of them provided with
the system, supported by the vendor or open source communities, but we can also
create our own from file systems or web pages. We'll be mostly dealing with the
defaults and how to enable or disable them, but there are many configurations that
can be made to customize software deployment.

```
# Checking how dnf is configured and seeing it’s available repositories
cat /etc/dnf/dnf.conf

[root@rocky14 ~]# cat /etc/dnf/dnf.conf
[main]
gpgcheck=1
installonly_limit=3
clean_requirements_on_remove=True
best=True
skip_if_unavailable=False

# has some interesting information about what is or isn’t going to be checked.
# You can include a line here called exclude= to remove packages from installation
# by name. Where a repo conflicts with this, this takes precedence.
dnf repolist
dnf history

[root@rocky14 ~]# dnf repolist
repo id                   repo name
appstream                 Rocky Linux 9 - AppStream
baseos                    Rocky Linux 9 - BaseOS
epel                      Extra Packages for Enterprise Linux 9 - x86_64
epel-cisco-openh264       Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64
extras                    Rocky Linux 9 - Extras

# Checking where repos are stored and what they look like
ls /etc/yum.repos.d/

[root@rocky14 ~]# ls  /etc/yum.repos.d
epel-cisco-openh264.repo  epel.repo          rocky-devel.repo   rocky.repo
epel-testing.repo         rocky-addons.repo  rocky-extras.repo

# Repos are still stored in /etc/yum.repos.d
cat /etc/yum.repos.d/rocky.repo
```

The mirror list system uses the connecting IP address of the client and the update
status of each mirror to pick current mirrors that are geographically close to the
client. You should use this for Rocky updates unless you are manually picking
other mirrors.

If the mirrorlist does not work for you, you can try the commented out baseurl line
instead.

```
[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever$rltype
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
countme=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-Rocky-9
#Output truncated for brevity’s sake….
```

Something you'll find out in the next section looking at repos is that when they are
properly defined they are enabled by default. enabled=1 is implied and doesn't need
to exist when you create a repo.

```
# Let’s disable a repo and see if the output changes at all
dnf config-manager --disable baseos

# Should now have the line enabled=0 (or false, turned off)
cat /etc/yum.repos.d/rocky.repo

[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever$rltype
# baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=0
countme=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-Rocky-9
# Output truncated for brevity’s sake….

# Re-enable the repo and verify the output
dnf config-manager --enable baseos

# Should now have the line enabled=1 (or true, turned back on)
cat /etc/yum.repos.d/rocky.repo

# Output:
[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever$rltype
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
countme=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-Rocky-9
# Output truncated for brevity’s sake….
```

DONE

### Installing software you were asked by an application team:

So someone has asked for some software and assured you it's been tested in similar
environments, so you go to install it on their system for them.

```
# See if we already have a version.
rpm -qa mariadb

# See if dnf knows about it
dnf search mariadb
dnf search all mariadb

# What is DNF showing you? What are the differences between these commands based on the output?

If the "--all" option is used, lists packages that match at least one of the keys (an OR operation)

# Try to install it
dnf install mariadb
# hit “N”

# Make note of any dependencies that are added on top of mariadb (there’s at least one)
mariadb-common
mariadb-connector-c
mariadb-connector-c-config
perl-Sys-Hostname

# What does DNF do with the transaction when you cancel it? Can you compare this
# to what you might have used before with YUM? How are they different?
# (You can look it up if you don’t know.)

It depends at what stage of the installation when you cancel it.

If before downloading, DNF and YUM both do the same, simply cancel.
If during downloading, DNF and YUM both do the same, downloads are canceled, partials are discarded.
If during installation, DNF will protect your installations by either rolling back
to the previous transaction in the transaction database, will remove partially
installed packages, and roll back changes. There is a low chance of any partially
installed packages.
YUM you can run into partailly installed packages, broken dependencies, and 
system inconsistencies.


# Ok, install it
dnf -y install mariadb

# Will just assume yes to everything you say.
# You can also set this option in /etc/dnf/dnf.conf to always assume yes,
# it’s just safer in an enterprise environment to be explicit.
```

### Removing packages with dnf:

Suprise, the user calls back because that install has made the system unstable.
They are asking for you to remove it and make the system back to the recent version.

```
dnf remove mariadb
# hit “N”

# this removes mariadb from your system
dnf -y remove mariadb

# But did this remove those dependencies from earlier?
rpm -q {dependency}
rpm -qi {dependency}

# How are you going to remove that if it’s still there?
# Checking where something came from. What package provides something in your system

# One of the most useful commands dnf provides is the ability to know “what provides”
# something. Sar and iostat are powerful tools for monitoring your system.
# Let’s see how we get them or where they came from, if we already have them.
# Maybe we need to see about a new version to work with a new tool.
dnf whatprovides iostat
dnf whatprovides sar

[root@rocky14 ~]# dnf whatprovides sar
Last metadata expiration check: 0:13:00 ago on Tue May 13 20:36:10 2025.
sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : @System
Matched from:
Filename    : /usr/bin/sar

sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : appstream
Matched from:
Filename    : /usr/bin/sar

# Try it on some other tools that you regularly use to see where they come from.
dnf whatprovides systemd
dnf whatprovides ls
dnf whatprovides python

[root@rocky14 ~]# dnf whatprovides python
Last metadata expiration check: 0:12:32 ago on Tue May 13 20:36:10 2025.
python-unversioned-command-3.9.21-1.el9_5.noarch : The "python" command that runs Python 3
Repo        : appstream
Matched from:
Provide    : python = 3.9.21-1.el9_5

# Using Dnf to update your system or individual packages
# Check for how many packages need update
dnf update

# How many packages are going to update?
3 install
45 upgrade
3 remove

# Is one of them the kernel?
Yes

# What is the size in MB that is needed?
18M for kernel-core
36M for kernel-modules

# Hit “N”

# Your system would have stored those in /var/cache/dnf
# Let’s check to see if we have enough space to hold those
df -h /var/cache/dnf

[root@rocky14 ~]# df -h /var/cache/dnf
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           8.0G  2.5G  5.6G  31% /

# Is there more free space than there is needed size in MB from earlier?
Yes
# There probably is, but this becomes an issue. You’d be surprised.

# Let’s see how that changes if we exclude the kernel
dnf update --exclude=kernel

Command needs to add a glob character after kernel*

Transaction Summary
=====================================================================================================================================================
Upgrade  45 Packages

Total download size: 515 M

# How many packages are going to update?
45

# Is one of them the kernel?
No

# What is the size in MB that is needed?
515

# Hit “N”

```

You can update your system if you like. You'd have to reboot for your system to
take the new kernel. If you do that you can redo the grubby portion and the 
ls /boot will show the new installaed kernel, uness you excluded it.

### Using dnf to install group packages:

Maybe we don't even know what we need to get a project going. We know that we need
to have a web server running but we don't have an expert around to tell use everything
that may help to make that stable. We can scour the interwebs (our normal job) but
we also have a tool that will gives us the base install needed for RHEL or CentOs
to run that server

```
dnf grouplist
dnf group install development

[root@rocky14 ~]# dnf grouplist
Last metadata expiration check: 0:18:44 ago on Tue May 13 20:36:10 2025.
Available Environment Groups:
   Server with GUI
   Server
   Minimal Install
   Workstation
   KDE Plasma Workspaces
   Custom Operating System
   Virtualization Host
Available Groups:
   Fedora Packager
   VideoLAN Client
   Xfce
   Legacy UNIX Compatibility
   Console Internet Tools
   Container Management
   Development Tools
   .NET Development
   Graphical Administration Tools
   Headless Management
   Network Servers
   RPM Development Tools
   Scientific Support
   Security Tools
   Smart Card Support
   System Tools

# How many packages are going to update?
Install  107 Packages
Upgrade    6 Packages

# Is one of them the kernel?
No

# What is the size in MB that is needed?
135 M

# Hit “N”
# Do you see a pattern forming?
Yea, we always check dependencies, size, and other aspects of the software we want
to install.
```

If you install this you're going to have developer tools installed on the server
but they won't be configured. How would you figure out what tools and versions
were just installed? How might you report this for your own documentation and
to a security team that keeps your security baselines?

Just running `dnf group install development` outputs all packages to the screen.
You can dump that output into a file and that file will now have all the packages
and their version numbers. You can then add that to your documentation.



