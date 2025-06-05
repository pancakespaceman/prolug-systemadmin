# Unit 10 - Kubernetes

## Overview

This unit introduces Kubernetes (K8s), an open-source container orchestration platform
that automates the deployment, scalling, and management of containerized applications.
This unit covers:
	- *Understanding Kubernetes Architecture* - Nodes, Control Planes, and Cluster Components.
	- *Installing K3s* - A lightweight Kubernetes distribution optimized for resource efficiency.
	- *Interacting with Kubernetes* - Using `kubectl` to manage and troubleshoot clusters.
	- *Deploying Applications* - Creating and managing *Pods, Deployments, and Services*
	- *Security and Best Practices* - Implementing security measures and troubleshooting issues.

Kubernetes play a critical role in *modern enterprise infrastructure*, enabling
*scalability, high availability, and automation* in cloud-nat ive applications.

## Learning Objectives

By the end of this unit, learners will:

1. Understand the Core Concepts of Kubernetes:
	- Define Kubernetes and explain its role in contain orchestration
	- Differentiate between Kubernetes vs. PaaS (Platform as a Service)
2. Deploy and Manage Kubernetes Clusters:
	- Install K3s and verify its functionality
	- Manage cluster resources using kubectl
3. Perform Basic Kubernetes Operations:
	- Create and manage Pods, Deployments, and Services
	- Understand the role of Namespaces, ConfigMaps, and Secrets
4. Troubleshoot Kubernetes Clusters:
	- Identify commmon cluster issues and validate node status.
	- Diagnose networking and pod scheduling problems
5. Apply security Best Practices in Kubernetes:
	- Secure containerized applications using best practices
	- Implement Kubernetes Pod Security Standards

## Relevance & Context

Kubernetes is a foundational technology in modern DevOps and cloud computing.
Understanding it is critical for system administrators, DevOps engineers, and
site reliability engineers (SREs) for several reasons:
	- *Scalability & Automation*: Automates containerized application deployments,
	scaling, and management.
	- *Resource Efficiency*: Optimizes workload distribution across multiple nodes.
	- *Infrastructure as Code (IaC)*: Kubernetes configurations can be defined
	declaratively using YAML.
	- *Cross-Cloud Compatibility*: Supports deployment across on-premises, hybrid,
	and multi-cloud environments.
	- *High Availability & Self-Healing*: Detects and replaces failed instances automatically.

## Prerequisites

Before beginning this unit, learners should have:

	- A working knowledge of Linux system administration.
	- Experience using the command line (bash, ssh, vim).
	- Familiarity with containers and tools like Docker.
	- Basic networking knowledge, including IP addressing and port management.

## Key Terms and Definitions

Kubernetes (K8s)

K3s

Control Plane

Nodes

Pods

Deployments

Services

Kubelet

Scheduler

ETCD

Kube-proxy

Static Pod

## Lecture Notes

K3s Setup (Kubernetes Light)
Kubernetes concepts
Looking at kubernetes resources
Deployhments in Kubernetes
	- Imperitive
	- Declaritive

### Why do we case as an Admin?

We're not the devs (in this role), so why do we care?

- We are responsible for installing the Kubernetes envrionment
- K8s or K3s may be used.
	- K8s has more moving parts and more production capabilities
	- K3s tends to be quick  and dirty to prove functionality and testing.
		- Remember MVP (minimum viable product) and PoC (proof of concept)
- We may have to help troublshoot production workloads
	
	






