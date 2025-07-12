# Unit 10 Lab - Kubernetes

https://killercoda.com/het-tanis/course/Kubernetes-Labs/three-tier-app-in-kubernetes

> If you are unable to finish the lab in the ProLUG lab environment we ask you `reboot`
> the machine from the command line so that other students will have the intended environment.

### Resources / Important Links

- [Killercoda Labs](https://killercoda.com/learn)
- [Kubernetes Documentation](https://kubernetes.io/docs/concepts/overview/)
- [K3s Official Site](https://k3s.io/)
- [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [Kubernetes Troubleshooting Guide](https://kubernetes.io/docs/tasks/debug/)

### Required Materials

- Rocky 9.4+ - ProLUG Lab
  - Or comparable Linux box
- root or sudo command access

## Pre-Lab: Quick Warmup and System Checks

---

Before installing K3s, verify system compatibility and gather initial data.

### Step 1: Download and Inspect K3s Installer

```sh
curl -sfL https://get.k3s.io > /tmp/k3_installer.sh
more /tmp/k3_installer.sh
```

#### Questions:

- What system checks does the installer perform?
	- if systemd or openrc is installed
	- system architecture
- What environment variables does it check?
	- release version
	- selinux version

### Step 2: System Architecture Check

```sh
uname -m
grep -i arch /tmp/k3_installer.sh
```

#### Questions:

- What is the variable holding the system architecture?
	- ARCH
- How does K3s determine system compatibility?
	- it uses a switch case to check if the system is a supported architecture

### Step 3: SELinux Status Check

```sh
grep -iE "selinux|sestatus" /tmp/k3_installer.sh
sestatus
```

#### Questions:

- Does K3s check if SELinux is enabled?
	- yes
- What are the implications of SELinux on Kubernetes deployments?
	- k3s will need to be included with the SELinux permissions in order for it to
	run nominally

## Installing K3s and Verifying the Service

### Step 4: Install K3s

```sh
curl -sfL https://get.k3s.io | sh -
```

the installer script also installed k3s-selinux

NOTE:
	- I disabled wwclient, firewalld, and selinux with `setenforce 0`
	- IGNORE FOLLOWING NOTES:
	- In order for some of the kubectl commands to work, I had to add an argument to the
	installation command to bind the default port to 127.0.0.1.
	`curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --bind-address=127.0.0.1" sh -`
	- A lot of my issues are that I installed k3s in agent mode. I need to install it in server mode.

### Step 5: Verify Installation

```sh
systemctl status k3s
systemctl is-enabled k3s
```

- What files and services were installed?
	- k3s-selinux
	- k3s.service
- Is K3s set to start on boot?
	- yes, it is set to enable

### Step 6: Explore System Services

```sh
systemctl cat k3s
```

- What startup configurations does K3s have?
	- nm-cloud=setup.service
	- modprobe br_netfilter
	- modprobe overlay
	- k3s server
- Does it rely on any dependencies?
	- k3s-selinux
	- i cant tell of any others from the systemctl cat k3s command

## Exploring Kubernetes Environment

### Step 7: Checking Kubernetes Components

```sh
kubectl version

[root@rocky13 ~]# kubectl version
Client Version: v1.32.6+k3s1
Kustomize Version: v5.5.0
Server Version: v1.32.6+k3s1


kubectl get nodes

[root@rocky13 ~]# kubectl get nodes
NAME      STATUS   ROLES                  AGE    VERSION
rocky13   Ready    control-plane,master   5m2s   v1.32.6+k3s1


kubectl get pods -A

[root@rocky13 ~]# kubectl get pods -A
NAMESPACE     NAME                                      READY   STATUS    RESTARTS      AGE
kube-system   coredns-5688667fd4-x578m                  0/1     Running   0             44s
kube-system   helm-install-traefik-crd-g5n26            0/1     Error     2 (21s ago)   44s
kube-system   helm-install-traefik-pjccg                0/1     Error     2 (21s ago)   44s
kube-system   local-path-provisioner-774c6665dc-cz49f   0/1     Error     2 (26s ago)   44s
kube-system   metrics-server-6f4c6675d5-t2vz2           0/1     Error     2 (24s ago)   44s


kubectl get namespaces

[root@rocky13 ~]# kubectl get namespaces
NAME              STATUS   AGE
default           Active   5m30s
kube-node-lease   Active   5m30s
kube-public       Active   5m30s
kube-system       Active   5m30s


kubectl get configmaps -A

[root@rocky13 ~]# kubectl get configmaps -A
NAMESPACE         NAME                                                   DATA   AGE
default           kube-root-ca.crt                                       1      74s
kube-node-lease   kube-root-ca.crt                                       1      74s
kube-public       kube-root-ca.crt                                       1      74s
kube-system       chart-content-traefik                                  0      75s
kube-system       chart-content-traefik-crd                              0      75s
kube-system       cluster-dns                                            2      79s
kube-system       coredns                                                2      76s
kube-system       extension-apiserver-authentication                     6      83s
kube-system       kube-apiserver-legacy-service-account-token-tracking   1      83s
kube-system       kube-root-ca.crt                                       1      74s
kube-system       local-path-config                                      4      76s


kubectl get secrets -A

[root@rocky13 ~]# kubectl get secrets -A
NAMESPACE     NAME                        TYPE                               DATA   AGE
kube-system   chart-values-traefik        helmcharts.helm.cattle.io/values   1      92s
kube-system   chart-values-traefik-crd    helmcharts.helm.cattle.io/values   0      92s
kube-system   k3s-serving                 kubernetes.io/tls                  2      96s
kube-system   rocky13.node-password.k3s   Opaque                             1      91s
```

#### Questions:

- What namespaces exist by default?
	- default, kube-node-lease, kube-public, kube-system
	
- What secrets are stored in the cluster?

[root@rocky13 ~]# kubectl get secrets -A
NAMESPACE     NAME                        TYPE                               DATA   AGE
kube-system   chart-values-traefik        helmcharts.helm.cattle.io/values   1      92s
kube-system   chart-values-traefik-crd    helmcharts.helm.cattle.io/values   0      92s
kube-system   k3s-serving                 kubernetes.io/tls                  2      96s
kube-system   rocky13.node-password.k3s   Opaque                             1      91s

## Deploying Applications: Pods, Services, and Deployments

### Step 8: Create a Simple Web Server Pod

```sh
kubectl run webpage --image=nginx
```

- Verify pod creation:
```sh
kubectl get pods

[root@rocky13 ~]# kubectl get pods
NAME      READY   STATUS              RESTARTS   AGE
webpage   0/1     ContainerCreating   0          39s


kubectl describe pod webpage
```

### Step 9: Deploy a Redis Database with Labels

```sh
kubectl run database --image=redis --labels=tier=database
```

- Verify labels:
  ```sh
  kubectl get pods --show-labels
  ```

### Step 10: Expose the Redis Database

```sh
kubectl expose pod database --port=6379 --name=redis-service --type=ClusterIP
```

- Verify service:
  ```sh
  kubectl get services
  ```

### Step 11: Create a Web Deployment with Replicas

```sh
kubectl create deployment web-deployment --image=nginx --replicas=3
```

- Check status:
  ```sh
  kubectl get deployments
  ```

### Step 12: Create a New Namespace and Deploy an App

```sh
kubectl create namespace my-test
kubectl create deployment redis-deploy -n my-test --image=redis --replicas=2
```

- Verify deployment:
  ```sh
  kubectl get pods -n my-test
  ```

## Troubleshooting Cluster Issues

Your team reports an issue with the cluster:

```sh
[root@Test_Cluster1 ~]# kubectl get nodes
NAME            STATUS      ROLES                AGE     VERSION
Test_Cluster1   Ready       control-plane,master 17h     v1.30.6+k3s1
Test_Cluster2   NotReady    worker               33m     v1.29.6+k3s1
Test_Cluster3   Ready       worker               17h     v1.28.6+k3s1
```

### Step 13: Investigate Node Health

```sh
kubectl describe node Test_Cluster2
kubectl get pods -A
```

- What errors do you notice?
- Is there a resource constraint or version mismatch?

### Step 14: Restart K3s and Check Logs

```sh
systemctl restart k3s
journalctl -xeu k3s
```

- What errors appear in the logs?
- Does restarting resolve the issue?

## Conclusion

---

At the end of this lab, you should:

✅ Have a fully operational K3s Kubernetes cluster.  
✅ Be able to deploy and expose containerized applications.  
✅ Know how to troubleshoot common Kubernetes errors.  
✅ Understand security best practices for Kubernetes deployments.

📌 Next Steps: Continue testing deployments, set up monitoring tools like Prometheus or Grafana, and explore Ingress Controllers to manage external access.

> Be sure to `reboot` the lab machine from the command line when you are done.