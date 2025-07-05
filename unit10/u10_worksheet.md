# Unit 10 Worksheet - Kubernetes

## Resources / Important Links

(Kubernetes Overview)[https://kubernetes.io/docs/concepts/overview/]
(K3s Offical Documentation)[https://k3s.io/]
(Kubernetes Security Best Practices)[https://kubernetes.io/docs/concepts/security/]
(Pod Security Standards)[https://kubernetes.io/docs/concepts/security/pod-security-standards/]
(Interactice Kubernetes Labs)[https://killercoda.com/]


## Discussion Posts

### 1

Read: (Kubernetes Overview)[https://kubernetes.io/docs/concepts/overview/]

1. What are the two most compelling reasons to implement Kubernetes in your organization?

```
The two reasons I really like are:
Self-healing: containers can be restarted if they fail, containers can be replaces,
containers that don't respond can be killed, and containers that arn't available
are not advertised until they are ready.

Automated rollouts and rollbacks: You can describe the desired state of containers,
and change the current state to the desired state.
```

2. The article states that Kubernetes is not a PaaS. What does that mean? How does
Kubernetes compare to a traditional PaaS?

```
Platform as a Service (PaaS): the service provider delivers and manages all the hardware
and software resources needed for application development. You write the code
and manage all the apps and data, but you do not have to manage or maintain the
software development platform. PaaS manages more resources higher up in the "stack"
to further reduce the operational burden on developers and IT operations teams.

Kubernetes operates at container level, not hardware level. So, while Kubernenets
provides some generally applicable reatures common to PaaS (such as deployment, scaling,
load balancing, etc.) Kubernentes is not monolithic and provides more configuration.

Kubernetes doesn't limit types of applications that are supported, does not deploy
source code or do any building of the application, does not dictate logging, monitoring
or alerting solutions, and several other differences from a PaaS solution.
```

### 2 

Scenario:

Your team is troubleshooting a Kubernetes cluster where applications are failing
to deploy. They send you the following output:

```
[root@Test_Cluster1 ~]# kubectl version
Client Version: v1.31.6+k3s3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.30.6+k3s1

[root@rocky15 ~]# kubectl get nodes
NAME            STATUS      ROLES                  AGE   VERSION
Test_Cluster1   Ready       control-plane,master   17h   v1.30.6+k3s1
Test_Cluster2   NotReady    worker                 33m   v1.29.6+k3s1
Test_Cluster3   Ready       worker                 17h   v1.28.6+k3s1

```

1. How would you validate the error?

```
I would use this command to get information about the node.

kubectl describe node Test_Cluster2
kubectl get pods -A
```

2. What do you suspect is causing the problem?

```
Without seeing more about the node, first glance shows that there could be an issue
with the versioning. There may be a problem with version 1.29.6.
```

3. Has someone already attempted to fix this problem? Why or why not?

```
Yes, there has been some interaction with the problem node. Its AGE is only
33m compared to the other nodes which are 17h.
```

### 3

Scenario:

You are the Network Operations Center (NOC) lead, and your team is responsible for
monitoring development, test, and QA Kubernetes clusters.

Write a basic cluster health check procedure for new NOC personnel.

```
0. Write down any and all anomalies, and escalate once health check is finished
if issues exist.

1. Ensure correct cluster connection and context
This confirms the API server is responding correctly and lists available
contexts. If multiple contexts are present, then this health check should be
performed on each context.
Run:
	- `kubectl config get-contexts`
		- Confirm all contexts are showing that should be.
	- `kubectl config use-context <insert_desired_context>`
		- connect to a context.
	- `kubectl cluster-info`

2. Check Node Health
Run:
	- `kubectl get nodes -o wide`
		- All Nodes should be in `Ready` state
		- Check uptime and other info for anomalies

3. Check Pod Health
Run:
	- `kubectl get pods --all-namesaces -o wide`
		- All pods should be `Running`

4. Check Resource Usage
Run:
	- `kubectl top nodes`
	- `kubectl top pods --all-namespaces`
		- Look for higher CPU or Memory usage than normal

5. Review Cluster Events
Run:
	- `kubectl get events --all-namespaces --sort-by='.metadata.creationTimestamp'`
		- Look for errors such as `OOMKilled`, `FailedScheduling`, `FailedMount`,
		and Node pressure warnings


```

1. What online resources did you use to figure this out?

```
(Apptio Kubernetes Health Check)[https://www.apptio.com/blog/kubernetes-health-check/]
(Kube By Example Health Checks)[https://kubebyexample.com/concept/health-checks]
(Kubernetes Offical Documentation)[https://kubernetes.io/docs/home/]
ChatGPT as well to tie things together.
```

2. What did you learn during this process?

```
There are three kinds of health checks (aka probes).
	- Liveness probes detect a non-responsive application.
	- Readiness probes ensure that a service is ready to receive traffic.
	- Startup probes identify and delay application startup until it's prepared
	to handle requests.


Kubernetes hurts my brain.
```

### Responses

[x] Post 1
[x] Post 2
[x] Post 3


## Key Terminology & Definitions

Define the following Kubernetes terms:

Kubernetes/K8s:
	- An open-source container orchestration platform that automates the deployment,
	scaling, and management of containerized applications.
		- manages workloads across clusters of machines
		- Supports rolling updates, self-healing, service discovery, and more.
	- https://kubernetes.io/docs/concepts/overview/

K3s:
	- A lightweight Kubernetes distribution designed for edge computing, IoT, and
	development environments. It is easy to install and optimized for low-resource
	environments.
		- ideal for single-node clusters and edge devices
	- https://k3s.io/

Control Plane:
	- the brain of a kubernetes cluster, responsible for global decisions like
	scheduling, maintaining cluster state, and responding to cluster events.
	- Components: kube-apiserver, etcd, kube-scheduler, kube-controller-manager
	- https://kubernetes.io/docs/concepts/overview/components/#control-plane-components

Node:
	- A physical or virtual machine that runs your containerized workloads. Each node
	includes:
		- `kubelet` (agent)
		- `kube-proxy` (networking)
		- Container runtime (Docker, containerd, etc.)
	- https://kubernetes.io/docs/concepts/architecture/nodes/

Pod:
	- the smallest deployable unit in Kubernetes. It encapsulates one or more containers
	that share:
		- Storage (volumes)
		- Network IP and port space
		- Lifecycle
	- https://kubernetes.io/docs/concepts/workloads/pods/

Deployment:
	- a Kubernetes controller that manages replica sets to keep your pods running
	in a desires state - handling rolling updates, restarts, and scaling
	- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

Service:
	- a stable networking abstraction that allows you to expose pods to each other
	or to the outside world
	- Types:
		- ClusterIP (internal)
		- NodePort
		- LoadBalancer
		- ExternalName
	- https://kubernetes.io/docs/concepts/services-networking/service/

ETCD:
	- a distributed key-value store used by Kubernetes to store all cluster configuration
	and state. It is the source of truth for the cluster.
	- https://etcd.io/docs/v3.5/

Kubelet:
	- an agent that runs on each node. It watches the API server and ensures
	that containers are running as described in the PodSpecs.
	- https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/

Kube-proxy:
	- maintains network rules on nodes, allowing communication between pods and 
	services. It handles traffic routing and fowards requests to appropriate pods.
	- https://kubernetes.io/docs/concepts/overview/components/#kube-proxy

Scheduler:
	- scheduler is a control plane component that assigns pods to nodes based on
	resource availability, affinity rules, tains, and other contraints
	- https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/

API Server:
	- kube-apiserver is the front end of the Kubernetes control plane. It handles
	all REST operations and acts as the central hub through which all other
	components interact.
	- https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver

## Lab and Assignment

Unit 10 Lab k3s

Continue working on your project from the Project Guide

Topics:

System Stability
System Performance
System Security
System monitoring
Kubernetes
Programming/Automation You will research, design, deploy, and document a system 
that improves your administration of Linux systems in some way.


## Digging Deeper

1. Build a custom container and deploy it in Kubernetes securely.

2. Read about container security:
	- (Docket Security Best Practices)[https://docs.docker.com/build/building/best-practices/]
	- (Pod Security Standards)[https://kubernetes.io/docs/concepts/security/pod-security-standards/]

3. Complete this Kubernetes security lab:

(KillerShell Kubernetes Security)[https://killercoda.com/killer-shell-cks/scenario/static-manual-analysis-k8s]

## Reflection Questions

1. What questions do you still have about Kubernetes?

2. How can you apply this knowledge in your current IT role?

3. If you’re not in IT, how could this experience contribute to your resume or portfolio?


