# Unit 5 Lab - Managing Users and Groups

## Resources

(Killercoda Labs)[https://killercoda.com/learn]
(RedHat: User and Group Management)[https://www.redhat.com/en/blog/linux-user-group-management]
(Rocky Linux User Admin Guide)[https://docs.rockylinux.org/books/admin_guide/06-users/]
(Killercoda lab by FishermanGuyBro)[https://killercoda.com/fishermanguybro]

stop warewulf for the lab

systemctl stop wwclient

## Pre-Lab Warm-Up

Exercises

1. `mkdir lab_users`
2. `cd /lab_users`
3. `cat /etc/passwd`
  - We'll be examining the contents of this file later
4. `cat /etc/passwd | tail -5`
  - What did this do to the output of the file?
  - This only grabs the last 5 lines of the file
5. `cat /etc/passwd | tail -5 | nl`
6. `cat /etc/passwd | tail -5 | awk -F : ‘{print $1, $3, $7}'`
  - What did that do and what do each of the $# represent?
    - columns in the file
  - Can you give the 2nd, 5th, and 6th fields?

```
[root@rocky7 lab_users]# cat /etc/passwd | tail -5 | awk -F : '{print $2, $5, $6}'
x  /home/labuser
x  /home/scott2
x  /home/fisherman
x  /var/ossec
x  /home/mysql
```

7. `cat /etc/passwd | tail -5 | awk -F : ‘{print $NF}'`
  - What does this $NF mean? Why might this be useful to us as administrators?
  - $NF gives the last field of the file
  - this specific command tells us how many users can actually login to the system.
  (with the tail -5 of course)
8. `alias`
  - Look at the things you have aliased.
  - These come from defaults in your .bashrc file. We'll configure these later
9. `cd /root`
10. `ls -l`
11. `ll`
  - Output should be similar.
12. `unalias ll`
13. `ll`
  - You shouldn't have this command available anymore.
14. `ls`
15. `unalias ls`
  - How did ls change on your screen?

No worries, there are two ways to fix the mess you've made. 
Nothing you've done is permanent, so logging out and reloading a shell 
(logging back in) would fix this.
We just put the aliases back.

16. `alias ll='ls -l --color=auto'`
17. `alias ls='ls --color=auto'`
  - Test with alias to see them added and also use ll and ls to see them work properly.


DONE


## Lab

This lab is designed to help you get familiar with the basics of the systems you
will be working on.

Some of you will find that you know the basic material but the techniques here
allow you to put it together in a more complex fashion.

It is recommended that you type these commands and do not copy and paste them.
Browsers sometimes like to format characters in a way that doesn't always play nice
with Linux.

### The Shadow password suite

There are 4 files that comprise of the shadow password suite. We'll investigate them
a bit and look at how they secure the system. The four files are `/etc/passwd`,
`/etc/group`, `/etc/shadow`, and `/etc/gshadow`.

1. Look at each of the files and see if you can determin some basic information about them.

```
more /etc/passwd
more /etc/group
more /etc/shadow
more /etc/gshadow
```

There is one other file you may want to become familiar with:

```
more /etc/login.defs
```

Check the file permissions:

```
ls -l /etc/passwd
```

Do this for each file to see how their permissions are set.

You may note that /etc/passwd and /etc/group are readable by everyone on the 
system but /etc/shadow and /etc/gshadow are not readable by anyone on the system.

2. Anatomy of the `/etc/passwd` file. `/etc/passwd` is broken down like this, a 
`:` (colon) delimited file:

| Username | Password | User ID (UID) | Group ID (GID) | User Info | Home Directory | Login Shell |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| puppet   | x        | 994      | 991      | Puppet server daemon | /opt/puppetlabs/server/data/puppetserver | /sbin/nologin |


`cat` or `more` the files to verify these are values you see.
Are there always 7 fields?

3. Anatomy of the `/etc/group` file `/etc/group` is broken down like this,
a : (colon) delimited file:

| Groupname | Password | Group ID | Group Members |
| --------- | -------- | -------- | ------------- |
| puppet    | x        | 991      | foreman, foreman-proxy |


`cat` or `more` the files to verify these are values you see.
Are there always 7 fields?


4. We're not going to break down the `g` files, but there are a lot of resources
online that can show you this same information. Suffice it to say, the passwords,
if they exist, are stored in a md5 digest format up to RHEL 5. RHEL 6, 7, 8, and 9
use SHA-512 hash. We cannot allow these to be read by just anyone because then
they could brute force and try to figure out our passwords.

### Creating and modifying local users

We should take a second to note that the systems you're using are tied into our
active directory with Kerberos. You will not be seeing your account in `/etc/passwd`,
as that authentication is occuring remotely. You can, however, run `id <username>`
to see user information about yourself that you have according to active
directory. Your `/etc/login.defs` file is default and contains a lot of the values
that control how our next commands work.

5. Creating Users

```
useradd user1
useradd user2
useradd user3
```

Do a quick check on our main files:

```
tail -5 /etc/passwd
tail -5 /etc/shadow

[root@rocky7 ~]# tail -5 /etc/passwd
wazuh:x:978:977::/var/ossec:/sbin/nologin
mysql:x:1004:1004::/home/mysql:/bin/bash
user1:x:1005:1005::/home/user1:/bin/bash
user2:x:1006:1006::/home/user2:/bin/bash
user3:x:1007:1007::/home/user3:/bin/bash
[root@rocky7 ~]# tail -5 /etc/shadow
sshd:!!:19882::::::
polkitd:!!:19910::::::
user1:!!:20208:0:99999:7:::
user2:!!:20208:0:99999:7:::
user3:!!:20208:0:99999:7:::
```

What UID and GID were each of these given? 
  - user1 has 1005 for UID and GID, user2 has 1006, and user3 has 1007
Do they match up? 
  - the UID and GID for each user matches
Verify your users all have home directories. Where would you check this?
  - the passwd file shows thier home directories. You can then just use `ls` to
  see if the directories exist.

```
ls /home
```

Your users /home/<username> directories have hidden files that were all pulled 
from a directory called /etc/skel. If you wanted to test this and verify you might
do something like this:

```
cd /etc/skel
vi .bashrc
```

Use vi commands to add the line:

```
alias dinosaur='echo "Rarw"'
```

Your file should now look like this:

```
# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi
alias dinosaur='echo "Rarw"'
# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=
# User specific aliases and functions
```

Save the file with :wq.

```
useradd user4
su - user4
dinosaur # Should roar out to the screen

[root@rocky7 skel]# useradd user17
[root@rocky7 skel]# su -user17
Try 'su --help' for more information.
[root@rocky7 skel]# su - user17
[user17@rocky7 ~]$ dinosaur
Rawr
```

Doing that changed the .bashrc file for all new users that have home directories
created on the server. An old trick, when users mess up their login files
(all the . files), is to move them all to a directory and pull them from `/etc/skel`
again. If the user can log in with no problems, you know the problem was something 
they created.

We can test this with the same steps on an existing user. 
Pick an existing user and verify they don't have that command

```
su - user1
dinosaur # Command not found
exit
```

Then, as root:

```
cd /home/user1
mkdir old_dot_files
mv .* old_dot_files          # Ignore the errors, those are directories
cp /etc/skel/.* /home/user1  # Ignore the errors, those are directories
su - user1
dinosaur # Should 'roar' now because the .bashrc file is new from /etc/skel
```

6. Creating groups from our `/etc/login.defs` we can see that the default range
for UIDs on this system, when created by useradd are:

```
UID_MIN 1000
UID_MAX 60000
```

So an easy way to make sure that we don't get confused on our group numbering is 
to ensure we create groups outside of that range. This isn't required, but can 
save you headache in the future.

```
groupadd -g 60001 project
tail -5 /etc/group

[root@rocky7 ~]# groupadd -g 60007 project
[root@rocky7 ~]# tail -5 /etc/group
user1:x:1005:
user2:x:1006:
user3:x:1007:
user17:x:1008:
project:x:60007:
```

You can also make groups the old fashioned way by putting a line right into the 
`/etc/group` file.

Try this:

```
vi /etc/group
```
  - `Shift+G` to go to the bottom of the file.
  - Hit `o` to create a new line and go to insert mode.
  - Add `project2:x:60002:user4`
  - Hit Esc
  - `:wq!` to write quit the file explicit force because it's a read only file.

```
id user 4 # Should now see the project2 in the user's groups

[root@rocky7 ~]# id user17
uid=1008(user17) gid=1008(user17) groups=1008(user17),60008(project2)
```

7. Modifying or deleting users. So maybe now we need to move our users into that group.

```
usermod -G project user4
tail -f /etc/group # Should see user4 in the group


[root@rocky7 ~]# usermod -G project user17
[root@rocky7 ~]# tail -f /etc/group
fisherman:x:1003:
wazuh:x:977:wazuh
mysql:x:1004:
user1:x:1005:
user2:x:1006:
user3:x:1007:
user17:x:1008:
project:x:60007:user17
project2:x:60008:
user4:x:1009:
```

But, maybe we want to add more users and we want to just put them in there:

```
vi /etc/group
```

  - `Shift+G` Will take you to the bottom.
  - Hit `i` (will put you into insert mode).
  - Add `,user1,user2` after `user4`.
  - Hit `Esc`.
  - `:wq` to save and exit.

Verify your users are in the group now

```
id user4
id user1
id user2

[root@rocky7 ~]# id user1
uid=1005(user1) gid=1005(user1) groups=1005(user1),60007(project)
[root@rocky7 ~]# id user2
uid=1006(user2) gid=1006(user2) groups=1006(user2),60007(project)
[root@rocky7 ~]# id user4
uid=1009(user4) gid=1009(user4) groups=1009(user4),60007(project)
```

8. Test group permissions. I included the permissions discussion from an earlier
lab because it's important to see how permissions affect what user can see what
information.

Currently we have `user1,2,4` belonging to group `project` but not `user3`. So
we will verify these permissions are enforced by the filesystem.

```
mkdir /project
ls -ld /project
chown root:project /project
chmod 775 /project
ls -ld /project


[root@rocky7 ~]# mkdir /project
[root@rocky7 ~]# ls -ld /project
drwxr-xr-x. 2 root root 40 Apr 30 14:14 /project
[root@rocky7 ~]# chown root:project /project
[root@rocky7 ~]# chmod 775 /project
[root@rocky7 ~]# ls -ld /project
drwxrwxr-x. 2 root project 40 Apr 30 14:14 /project
```

If you do this, you now have a directory `/project` and you've changed the group
ownership to `/project`. You've also given group `project` users the ability to
write into your directory. Everyone can still read from your directory.
Check permissions with users:

```
su - user1
cd /project
touch user1
exit
su - user3
cd /project
touch user3
exit


[root@rocky7 ~]# su - user1
Last login: Wed Apr 30 14:15:45 MST 2025 on pts/0
Last failed login: Wed Apr 30 14:16:25 MST 2025 on pts/0
There were 2 failed login attempts since the last successful login.
su: warning: cannot change directory to /home/user1: Permission denied
-bash: /home/user1/.bash_profile: Permission denied
[user1@rocky7 root]$ cd /project
[user1@rocky7 project]$ touch user1
[user1@rocky7 project]$ exit
logout
-bash: /home/user1/.bash_logout: Permission denied
[root@rocky7 ~]# su - user3
su: warning: cannot change directory to /home/user3: Permission denied
-bash: /home/user3/.bash_profile: Permission denied
[user3@rocky7 root]$ cd /project
[user3@rocky7 project]$ touch user3
touch: cannot touch 'user3': Permission denied
[user3@rocky7 project]$ exi
-bash: exi: command not found
[user3@rocky7 project]$ exit
logout
-bash: /home/user3/.bash_logout: Permission denied
```

Anyone not in the project group doesn't have permissions to write a file into that directory. Now, as the root user:

```
chmod 770 /project
```

Check permissions with users:

```
su - user1
cd /project
touch user1.1
exit
su - user3
cd /project # Should break right about here
touch user3
exit

[root@rocky7 ~]# su - user1
Last login: Wed Apr 30 14:16:31 MST 2025 on pts/0
su: warning: cannot change directory to /home/user1: Permission denied
-bash: /home/user1/.bash_profile: Permission denied
[user1@rocky7 root]$ cd /project
[user1@rocky7 project]$ touch user1.1
[user1@rocky7 project]$ ls
user1  user1.1
[user1@rocky7 project]$ exit
logout
-bash: /home/user1/.bash_logout: Permission denied
[root@rocky7 ~]# su - user3
Last login: Wed Apr 30 14:16:45 MST 2025 on pts/0
su: warning: cannot change directory to /home/user3: Permission denied
-bash: /home/user3/.bash_profile: Permission denied
[user3@rocky7 root]$ cd /project
-bash: cd: /project: Permission denied
[user3@rocky7 root]$ exit
logout
-bash: /home/user3/.bash_logout: Permission denied
```
You can play with these permissions a bit, but there's a lot of information 
online to help you understand permissions better if you need more resources.


### Working with permissions

Permissions have to do with who can or cannot access (read), edit (write), or
execute files.
Permissions look like this:

```
ls -l
```

| Permission | # of Links | UID Owner | Group Owner | Size (b) | Creation Month | Creation Day | Creation Time | Name of File |
| ---------- | ---------- |---------- |---------- |---------- |---------- |---------- |---------- |---------- |
| -rw-r--r--. | 1 | scott | domain_users | 58 | Jun | 22 | 08:52 | datefile |


The primary permissions comands we're going to use are going to be `chmod` (access)
and `chown` (ownership).

A quick rundown of how permissions break out:

| R   W   X   | R   W   X   | R   W   X   |
| 4   2   1   | 4   2   1   | 4   2   1   |
| 2^2 2^1 2^0 | 2^2 2^1 2^0 | 2^2 2^1 2^0 |
| Owner       | Group       | Everyone    |

Let's examine some permissions and see if we can't figure out what
permissions are allowed:

```
ls -ld /home/scott/
drwx------. 5 scott domain_users 4096 Jun 22 09:11 /home/scott/
```

The first character lets you know if the file is a directory, file, or link.
In this case we are looking at my home directory.

`rwx`: For UID (me)
  - What permissions do I have?
  - you can read, write, and execute

`---`: For Group
  - Who are they?
    - users who are part of the same group as you. domain_users
  - What can my group do?
    - nothing


`---`: For everyonen else
  - What can everyone else do?
    - nothing

Go find some other interesting files or directories and see what you see there.
Can you identify their characteristics and permissions?


