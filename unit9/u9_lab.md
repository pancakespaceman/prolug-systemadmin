# Unit 9 Lab - Containerization on Linux

> If you are unable to finish the lab in the ProLUG lab environment we ask you `reboot`
> the machine from the command line so that other students will have the intended environment.

### Resources / Important Links

- [Killercoda Labs](https://killercoda.com/learn)
- [https://podman.io/docs](https://podman.io/docs)
- [https://docs.docker.com/build/concepts/dockerfile/](https://docs.docker.com/build/concepts/dockerfile/)
- [https://docs.podman.io/en/latest/markdown/podman-exec.1.html](https://docs.podman.io/en/latest/markdown/podman-exec.1.html)

### Required Materials

- Rocky 9.4+ - ProLUG Lab
  - Or comparable Linux box
- root or sudo command access

#### Downloads

The lab has been provided for convenience below:

- <a href="./assets/downloads/u9/u9_lab.pdf" target="_blank" download>📥 u9_lab(`.pdf`)</a>
- <a href="./assets/downloads/u9/u9_lab.docx" target="_blank" download>📥 u9_lab(`.docx`)</a>

## Pre-Lab Warmup

---

1. `which podman`

[root@rocky13 ~]# which podman
/usr/bin/podman

2. `dnf whatprovides podman`

[root@rocky13 ~]# dnf whatprovides podman
Waiting for process with pid 1040 to finish.
Last metadata expiration check: 0:00:02 ago on Fri Jul  4 20:35:01 2025.
podman-4:5.2.2-13.el9_5.x86_64 : Manage Pods, Containers and Container Images
Repo        : @System
Matched from:
Provide    : podman = 4:5.2.2-13.el9_5

podman-5:5.4.0-10.el9_6.x86_64 : Manage Pods, Containers and Container Images
Repo        : appstream
Matched from:
Provide    : podman = 5:5.4.0-10.el9_6

3. `rpm -qi podman`  
When was this installed?
	- Mon Feb 10 18:37:44
	
What version is it?  
	- 5.2.2

Why might this be important to know?
	- when problems come up, knowing when a software was installed and the version
	number can help with  debugging the issue.

4. `podman images`

5. `podman ps`

[root@rocky13 ~]# podman images
REPOSITORY  TAG         IMAGE ID    CREATED     SIZE
[root@rocky13 ~]# podman ps
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES


What do you learn from those two commands?
	- podman images shows locally stored images
	- podman ps shows the list of running containers on the system
	- right now there are no local images nor running containers

Why might it be important to know on a system?
	- you need to know what images are availble and what containers are running

## Lab 🧪

---

### Building and running containers

Your tasks in this lab are designed to get you thinking about how container
deployments interact with our Linux systems that we support.

1. Pull and run a container

```bash
podman run -dt -p 8080:80/tcp docker.io/library/httpd
```

What do you see on your screen as this happens?

[root@rocky13 ~]# podman run -dt -p 8080:80/tcp docker.io/library/httpd
Trying to pull docker.io/library/httpd:latest...
Getting image source signatures
Copying blob 3da95a905ed5 done   |
Copying blob 816d153af128 done   |
Copying blob 1a50d37c990c done   |
Copying blob 4f4fb700ef54 done   |
Copying blob d812016dda12 done   |
Copying blob 365277d54dac done   |
Copying config e6af1d8a44 done   |
Writing manifest to image destination
1121c2f4e4fef3d80777ec58342112cf5ed58304342e2c8c215e5e8e31f8e5fc

2. Check your images again (from your earlier exercises)

```bash
podman images
```

Is there a new image, and if so, what do you notice about it?

[root@rocky13 ~]# podman images
REPOSITORY               TAG         IMAGE ID      CREATED       SIZE
docker.io/library/httpd  latest      e6af1d8a44e5  5 months ago  152 MB

	- yes there is a new image. It was created 5 months ago and is 152 MB in size

3. Check your podman running containers

```bash
podman ps
```

What appears to be happening? Can you validate this with your Linux knowledge?

[root@rocky13 ~]# podman ps
CONTAINER ID  IMAGE                           COMMAND           CREATED         STATUS         PORTS                         NAMES
1121c2f4e4fe  docker.io/library/httpd:latest  httpd-foreground  22 seconds ago  Up 22 seconds  0.0.0.0:8080->80/tcp, 80/tcp  zen_yonath

	- the pod is runing using port 8080
	- we can use ss -ntulp to see that there is a process with the user of conmon running on 
	port 8080

```bash
ss -ntulp
curl 127.0.0.1:8080

[root@rocky13 ~]# curl 127.0.0.1:8080
<html><body><h1>It works!</h1></body></html>
```

4. Inspect the running pod

```bash
podman inspect -l
```

What format is the output in?  
	- JSON
What important information might you want from this in the future?
	- args used to run the command, the current container state, the name of the image
	the container was spawned from, network settings, and more

```bash
podman logs -l
```

What info do you see in the logs?  
	- process output that normally goes to standard output? it has the timestamps
	for when the container receives an http request.
	
Do you see your connection attempt from earlier? What is the return code and
why is that important for troubleshooting?
	- yes
	- the status code of the request is 200. that status code is necessary to help
	debug issues with the http request/response cycle.

```bash
podman top -l
```

What processes is the pod running?  
	- httpd with the arg `-DFOREGROUND`
	
What other useful information might you find here?  
	- the child processes that the parent process spawned.
	- the runtime of the processes
	- the user who started the container

Why might it be good to know the user being run within the pod?
	- who started the container? do they have proper permissions to run the container?

5. Stop the pod by its name

```bash
podman stop <podname>

[root@rocky13 ~]# podman stop 1121c2f4e4fe
1121c2f4e4fe
```

Can you verify it is stopped from your previous commands?

```bash
podman ps
ss -ntulp
curl 127.0.0.1:8080


[root@rocky13 ~]# podman ps
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
[root@rocky13 ~]# ss -ntulp
Netid State  Recv-Q Send-Q Local Address:Port   Peer Address:Port Process
udp   UNCONN 0      0            0.0.0.0:111         0.0.0.0:*     users:(("rpcbind",pid=787,fd=5),("systemd",pid=1,fd=37))
udp   UNCONN 0      0               [::]:111            [::]:*     users:(("rpcbind",pid=787,fd=7),("systemd",pid=1,fd=39))
tcp   LISTEN 0      4096         0.0.0.0:111         0.0.0.0:*     users:(("rpcbind",pid=787,fd=4),("systemd",pid=1,fd=36))
tcp   LISTEN 0      128          0.0.0.0:22          0.0.0.0:*     users:(("sshd",pid=892,fd=3))
tcp   LISTEN 0      4096            [::]:111            [::]:*     users:(("rpcbind",pid=787,fd=6),("systemd",pid=1,fd=38))
tcp   LISTEN 0      128             [::]:22             [::]:*     users:(("sshd",pid=892,fd=4))
[root@rocky13 ~]# curl 127.0.0.1:8080
curl: (7) Failed to connect to 127.0.0.1 port 8080: Connection refused
```

Does the container still exist? Why might you want to know this?
	- the container is no longer listed. 
	- if container still exists, it could be hung up and not stopping properly,
	or it could continue to try to run despite the command for it to stop.

```bash
podman image
```

### Build an application in a container

The ProLUG lab will already have a version of this setup for you to copy and run.
If you are in a different environment, follow https://docs.docker.com/build/concepts/dockerfile/
for the general same steps.

1. Setup your lab environment

```bash
[root@rocky11 stream]# cd /lab_work/
[root@rocky11 lab_work]# ls
[root@rocky11 lab_work]# mkdir scott_lab9
[root@rocky11 lab_work]# cd scott_lab9/
[root@rocky11 scott_lab9]# ls
[root@rocky11 scott_lab9]# cp /labs/lab9.tar.gz .
[root@rocky11 scott_lab9]# tar -xzvf lab9.tar.gz
lab9/
lab9/Dockerfile
lab9/hello.py
[root@rocky11 scott_lab9]# ls
lab9 lab9.tar.gz
[root@rocky11 scott_lab9]# cd lab9
[root@rocky11 lab9]# pwd
/lab_work/scott_lab9/lab9
[root@rocky11 lab9]# ls
Dockerfile hello.py

[root@rocky13 shawn_lab9]# pwd
/root/lab_work/shawn_lab9
[root@rocky13 shawn_lab9]# ls
lab9  lab9.tar.gz
[root@rocky13 shawn_lab9]# cd lab9
[root@rocky13 lab9]# ls
Dockerfile  hello.py
```

2. Create a docker image from the docker file:

```bash
time podman build -t scott_hello .
#Use your name
```

What output to your screen do you see as you build this?  
Approximately how long did it take?

[root@rocky13 lab9]# time podman build -t shawn_hello .
STEP 1/1: FROM ubuntu:22.04
Resolved "ubuntu" as an alias (/etc/containers/registries.conf.d/000-shortnames.conf)
Trying to pull docker.io/library/ubuntu:22.04...
Getting image source signatures
Copying blob e735f3a6b701 done   |
Copying config 1b668a2d74 done   |
Writing manifest to image destination
COMMIT shawn_hello
--> 1b668a2d748d
Successfully tagged localhost/shawn_hello:latest
Successfully tagged docker.io/library/ubuntu:22.04
1b668a2d748d748b0460ac95024ec80af7b663ede9416c235a9c102556bc1780

real    0m7.173s
user    0m6.648s
sys     0m2.097s


If this breaks in the lab, how might you fix it? What do you suspect?
	- TODO

3. Verify that you have built the container

```bash
podman images

[root@rocky13 lab9]# podman images
REPOSITORY                TAG         IMAGE ID      CREATED       SIZE
localhost/shawn_hello     latest      1b668a2d748d  2 weeks ago   80.4 MB
docker.io/library/ubuntu  22.04       1b668a2d748d  2 weeks ago   80.4 MB
docker.io/library/httpd   latest      e6af1d8a44e5  5 months ago  152 MB
```

4. Run the container as a daemon

```bash
podman run -dt localhost/scott_example
```

5. Verify the name and that it is running

```bash
podman ps

[root@rocky13 lab9]# podman images
REPOSITORY                TAG         IMAGE ID      CREATED       SIZE
localhost/shawn_hello     latest      1b668a2d748d  2 weeks ago   80.4 MB
docker.io/library/ubuntu  22.04       1b668a2d748d  2 weeks ago   80.4 MB
docker.io/library/httpd   latest      e6af1d8a44e5  5 months ago  152 MB
[root@rocky13 lab9]# podman run -dt localhost/shawn_hello
255c49470aa5a918efadc48b0b59caf247c65f34355cb31fe2da95b7654ea2b1
[root@rocky13 lab9]# podman ps
CONTAINER ID  IMAGE                         COMMAND     CREATED        STATUS        PORTS       NAMES
255c49470aa5  localhost/shawn_hello:latest  /bin/bash   4 seconds ago  Up 4 seconds              pedantic_burnell
```

6. Exec into the pod and see that you are on the Ubuntu container

```bash
podman exec -it festive_pascal sh
cat /etc/*release
exit

[root@rocky13 lab9]# podman exec -it pedantic_burnell sh
# cat /etc/*release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.5 LTS"
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
# exit
```

### Conclusion

There are a lot of ways to use these tools. There are a lot of ways you will support them.
At the end of the day you're a Linux System Administrator, you're expected to understand
everything that goes on in your system. To this end, we want to know the build process and
run processes so we can help the engineers we support keep working in a Linux environment.

> Be sure to `reboot` the lab machine from the command line when you are done.

