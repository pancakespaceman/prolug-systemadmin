# Unit 9 Worksheet - Containerization on Linux

## Resources / Important Links

(Dockerfile reference)[https://docs.docker.com/reference/dockerfile/]
(Podman reference)[https://docs.podman.io/en/latest/Commands.html]


## Discussion Posts

### 1

It's a slow day in the NOC and you have heard that a new system of container
delpoyments are being used by your developers. Do some reading about containers,
docker, and podman.

1. What resources helped you answer this?

```
ChatGPT for some overview, 
this red hat page for further reading https://www.redhat.com/en/topics/containers
this video about what is containerization https://www.youtube.com/watch?v=0qotVMX-J5s&ab_channel=IBMTechnology
```

Notes:

Hardware > Host OS > Hypervisor > VM
	- VM has its own OS (called a guest OS) and libraries
		- If we want multiple instances, we'll need multiple copies of the OS and libs
	- issues can arise if the guest OS is different than the OS you dev on
	
Manifest (a file that describes the container) > Create an image > Actual continer

Continer contains all libs and runtimes needed to run an applcation

Host OS > Runtime Engine (docker, podman) > Containers
	- Containers don't use guest OS, just the libs needed for the app
		- if multiple isntances, less resources are used



2. What did you learn about that you didn't know before?

```
Containers are a way to run and manage multiple applications, and multiple copies
of those applications simultaneously.
	- The idea of containers allows for parts of a larger project to be worked on
	and either modified or replaced.
	- Containers don't need to know what other containers are doing, only what
	input/output they need/give.
	- Containers differ from virtual machines
		- VMs emulate hardware with an entire OS and libs, containers do not have anything
		outside what the application requires

Containers contain everything needed to run a given application.
	- all libs, runtimes, etc
	- allows for the applciation to be depoloyed more easily across platforms.
```

3. What seems to be the major benefit of containers?

```
As I wrote in the answer to the previous question, containers are isolated from each other.
This means that they can be independantly worked on, modified, or even replaced.
Also, since each container contains everything needed for the application to run,
deploying the application on different platforms is much easier.
```

4. What seems to be some obstacles to container deployment?

```
Some obstacles include:
	- Security: the containers share the host kernel
	- Complexity: the more containers, the hard to manage resources and other aspects
	- Network configuration: there can be issues with connectivity of the containers
	- Testing: you can't just ssh into a container, so you need special tools to
	keep track of logs and to test containers.
```


### 2

You get your first ticket about a problem with containers. One of the engineers is trying
to move his container up to the Dev environment shared server. He sends you over
this information about the command he's trying to run.

```
[developer1@devserver read]$ podman ps
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
[developer1@devserver read]$ podman images
REPOSITORY                TAG                IMAGE ID      CREATED      SIZE
localhost/read_docker     latest             2c0728a1f483  5 days ago   68.2 MB
docker.io/library/python  3.13.0-alpine3.19  9edd75ff93ac  3 weeks ago  47.5 MB
[developer1@devserver read]$ podman run -dt -p 8080:80/tcp docker.io/library/httpd

```
You decide to check out the server

```
[developer1@devserver read] ss -ntulp
Netid   State    Recv-Q   Send-Q      Local Address:Port        Peer Address:Port         Process
udp     UNCONN   0        0           127.0.0.53%lo:53               0.0.0.0:*             users:(("systemd-resolve",pid=166693,fd=13))
tcp     LISTEN   0        80              127.0.0.1:3306             0.0.0.0:*             users:(("mariadbd",pid=234918,fd=20))
tcp     LISTEN   0        128               0.0.0.0:22               0.0.0.0:*             users:(("sshd",pid=166657,fd=3))
tcp     LISTEN   0        4096        127.0.0.53%lo:53               0.0.0.0:*             users:(("systemd-resolve",pid=166693,fd=14))
tcp     LISTEN   0        4096                    *:8080                   *:*             users:(("node_exporter",pid=662,fd=3))

```

-d flag means detach: run the container in the background and print the new container ID
-t flag means tty: allocate a pseudo-TTY
-p means publis: Publish a container’s port, or range of ports, to the host.
	- [[ip:][hostPort]:]containerPort[/protocol]
	- by not mentioning the IP addr, it binds all interfaces
podman run should pull the image from the repository if its not found locally

1. What do you think the problem might be?
	- How will you test this?
	
```
The command used by the dev is trying to use port 8080 for the httpd container. However,
that port is already in use by node exporter.
A simplle test would be to try to run the command with a different port.
```

2. The devleoper tells you that he's pulling a local image. Do you find this to be
true, or is something else happening in their `run` command?

```
No, the dev is not using a local image. The podman run command will pull from 
the repository if the image is not found locally.
```

### Responses

[x] Post 1
[x] Post 2


## Definitions

Container:
	- A lightweight, standalone, and executable unit of software that includes
	application code, system tools, libraries, and dependencies.
	- Everything is bundled together and isolated from the host sytem using Linux
	kernel features like:
		- Namespaces (process, network, mount, etc)
		- Cgroups (resource control)
	- Containers are not VMs, they share the host kernel.
	- Containers offer portability, isolation, efficiency, and consistency

Docker:
	- A platform that allows you to develop, ship, and run applications in containers.
	- https://docs.docker.com/get-started/docker-overview/

Podman:
	- A daemonless container engine developed by Red Hat that can run and manage
	OCI containers. Its a drop-in replacement for Docker in many cases and supports
	rootless containers and systemd integration.

CI/CD:
	- Continuous Integration / Continuous Deployment (or Delivery)
	- a software engineering pratice that automates building, testing, and deploying code.
		- CI: Developers integrate code into a shared repo frequently. Automated
		tests run to catch bugs early
		- CD: Code changes are automatically delivered or deployed to production

Dev/Prod Environments (Development/Production Environments):
	- separate environments used during software deployment
	- Development: Where developers build and test new features. Can include
	debugging tools, verbose logging, and in-progres code.
	- Production: the live environment that end users access. Must be stable, secure,
	and performant.

Dockerfile:
	- A script that contains instructions for building a Docker image. It defines
	what base image to use, what files to copy, what commands to run, and what ports
	to expose.

Docker/Podman images:
	- an image is a read-only template used to create containers. It includes the
	application, libraries, configuration files, environment variables, and metadata.
	- Docker and Podman both use OCI-compliant images
	- Images can be layered and cached for reuse.

Repository:
	- a storage location for container images. It can be public or private.

## Digging Deeper

1. Find a blog on deployment of some service or application in a container that 
interests you. See if you can get the deployment working in the lab.
	- What worked well?
	- What did you have to troubleshoot?
	- What documentation can you make to be able to do this faster next time?


2. Run this scenario and play with K3s: https://killercoda.com/k3s/scenario/intro

## Reflection Questions

1. What questions do you still have about this week?

2. How can you apply this now in your current role in IT? If you're not in IT, how
can you look to put something like this into your resume or portfolio?


