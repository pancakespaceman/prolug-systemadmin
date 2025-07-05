# Unit 10 Bonus - Kubernetes

> **NOTE:** This is an **optional** bonus section. You **do not** need to read it, but if you're interested in digging deeper, this is for you.

This section provides **advanced troubleshooting techniques**, **security best practices**, and **real-world challenges** to strengthen your Kubernetes knowledge.

## Step 1: Troubleshooting Kubernetes Cluster Issues

---

When things go wrong, **systematic troubleshooting** is key. Here’s how you diagnose **common Kubernetes issues**.

### Node Not Ready

**Check node status**

```sh
kubectl get nodes
kubectl describe node <node-name>
```

**Investigate Kubelet logs**

```sh
journalctl -u k3s -n 50 --no-pager
```

**Verify system resources**

```sh
free -m     # Check available memory
df -h       # Check disk space
htop        # Monitor CPU usage
```

**Possible Fixes**

- Restart K3s on the failing node:
  ```sh
  systemctl restart k3s
  ```
- Ensure network connectivity:
  ```sh
  ping <control-plane-ip>
  ```

### Pods Stuck in "Pending" or "CrashLoopBackOff"

**Check pod status**

```sh
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

**Possible Fixes**

- If **insufficient resources**, scale up the cluster.
- If **missing images**, check container registry authentication.
- If **misconfigured storage**, inspect volumes:
  ```sh
  kubectl get pvc
  ```

## Step 2: Securing Kubernetes Deployments

---

Security is crucial in enterprise environments. Here are **quick wins** for a more **secure Kubernetes cluster**.

### Limit Pod Privileges

**Disable privileged containers**

```yaml
securityContext:
  privileged: false
```

**Enforce read-only file system**

```yaml
securityContext:
  readOnlyRootFilesystem: true
```

### Restrict Network Access

**Use Network Policies to restrict pod communication**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
    - Ingress
```
Use [NetworkPolicy Editor](https://editor.networkpolicy.io/) to create and edit your network policies.

### Use Pod Security Admission (PSA)

Enable PSA to enforce security levels:

```sh
kubectl label --overwrite ns my-namespace pod-security.kubernetes.io/enforce=restricted
```

## Step 3: Performance Optimization Tips

---

**Enhance Kubernetes efficiency with these quick optimizations**:

### Optimize Resource Requests & Limits

Set appropriate **CPU & Memory limits** in deployments:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

**Why?** Prevents a single pod from consuming excessive resources.

### Enable Horizontal Pod Autoscaling (HPA)

Auto-scale pods **based on CPU or memory usage**:

```sh
kubectl autoscale deployment my-app --cpu-percent=50 --min=2 --max=10
```

## Step 4: Bonus Challenge - Build a Secure, Scalable App

---

**Challenge:**

- **Create a secure containerized app**
- **Deploy it in Kubernetes**
- **Implement Network Policies**
- **Apply Pod Security Standards**

**Helpful Resources**:

- [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [Kubernetes Hardening Guide](https://www.cisa.gov/kubernetes-hardening-guide)
- [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/)

## Conclusion

---

This **bonus section** strengthens **your Kubernetes troubleshooting, security, and performance tuning skills**. Apply these principles in real-world deployments!
 
## Downloads