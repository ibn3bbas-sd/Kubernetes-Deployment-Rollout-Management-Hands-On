# 🚀 Kubernetes Deployment Rollout Management

A **comprehensive, hands-on guide** to managing Kubernetes deployments, covering rollout strategies, version tracking, and safe rollback procedures using real-world examples.

---

## 📋 Table of Contents

* 📌 [Overview](#overview)
* ⚙️ [Prerequisites](#prerequisites)
* 🏗️ [Creating Your First Deployment](#creating-your-first-deployment)
* 📊 [Monitoring Rollout Status](#monitoring-rollout-status)
* 🕘 [Viewing Rollout History](#viewing-rollout-history)
* 🔄 [Updating Deployments](#updating-deployments)
* 📝 [Recording Changes](#recording-changes)
* ⏪ [Rolling Back Changes](#rolling-back-changes)
* ✅ [Best Practices](#best-practices)
* 📖 [Common Commands Reference](#common-commands-reference)
* 🛠️ [Troubleshooting](#troubleshooting)
* 🤝 [Contributing](#contributing)
* 📄 [License](#license)
* 👤 [Author](#author)

---

## 📌 Overview

This repository demonstrates **practical Kubernetes deployment management techniques** with a strong focus on:

* Safe rollout strategies
* Deployment versioning
* Rollback mechanisms

All examples are based on **real-world scenarios** using **nginx** as the sample application.

---

## ⚙️ Prerequisites

Before getting started, ensure you have the following:

* ✅ Kubernetes cluster (**v1.19+**)
* ✅ `kubectl` CLI installed and configured
* ✅ Basic understanding of Kubernetes concepts (Pods, Deployments, ReplicaSets)

---

## 🏗️ Creating Your First Deployment

Create a simple Kubernetes Deployment using the **nginx** image:

```bash
kubectl create deployment nginx --image=nginx:1.16
```

**Expected output:**

```
deployment.apps/nginx created
```

---

## 📊 Monitoring Rollout Status

Track the progress of your deployment rollout:

```bash
kubectl rollout status deployment nginx
```

**Expected output:**

```
Waiting for deployment "nginx" rollout to finish: 0 of 1 updated replicas are available...
deployment "nginx" successfully rolled out
```

---

## 🕘 Viewing Rollout History

### 🔍 Basic History View

Display the rollout history of the deployment:

```bash
kubectl rollout history deployment nginx
```

**Expected output:**

```
deployment.apps/nginx
REVISION  CHANGE-CAUSE
1         <none>
```

---

### 🧾 Detailed Revision Information

Inspect a specific revision using the `--revision` flag:

```bash
kubectl rollout history deployment nginx --revision=1
```

**Expected output:**

```
deployment.apps/nginx with revision #1
Pod Template:
  Labels:       app=nginx
                pod-template-hash=78449c65d4
  Containers:
   nginx:
    Image:      nginx:1.16
    Port:       <none>
    Host Port:  <none>
    Environment: <none>
    Mounts:     <none>
  Volumes:      <none>
```

---

## 🔄 Updating Deployments

### 🔧 Method 1: Using `kubectl set image`

Update the container image version:

```bash
kubectl set image deployment nginx nginx=nginx:1.17 --record
```

⚠️ **Note:** The `--record` flag is deprecated but still functional. It stores the executed command in the rollout history.

---

### ✏️ Method 2: Using `kubectl edit`

Edit the deployment manifest directly:

```bash
kubectl edit deployments.apps nginx --record
```

This opens your default editor. Modify the image version, save, and exit.

**Verify the update:**

```bash
kubectl rollout history deployment nginx
```

**Output:**

```
deployment.apps/nginx
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx nginx=nginx:1.17 --record=true
3         kubectl edit deployments.apps nginx --record=true
```

---

## 📝 Recording Changes

Recording changes helps track **what changed and why**. The `--record` flag captures the command used for each revision in the CHANGE-CAUSE field:

```bash
kubectl set image deployment nginx nginx=nginx:1.17 --record
```

**Verify the recorded change:**

```bash
kubectl rollout history deployment nginx
```

**Output:**

```
deployment.apps/nginx
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx nginx=nginx:1.17 --record=true
```

**View detailed information about a specific revision:**

```bash
kubectl rollout history deployment nginx --revision=3
```

**Output:**

```
deployment.apps/nginx with revision #3
Pod Template:
  Labels:       app=nginx
                pod-template-hash=787f54657b
  Annotations:  kubernetes.io/change-cause: kubectl edit deployments.apps nginx --record=true
  Containers:
   nginx:
    Image:      nginx:latest
    Port:       <none>
    Host Port:  <none>
    Environment: <none>
    Mounts:     <none>
  Volumes:      <none>
```

---

## ⏪ Rolling Back Changes

### 🔙 Rollback to the Previous Revision

Undo the most recent change:

```bash
kubectl rollout undo deployment nginx
```

**Verify the rollback:**

```bash
kubectl describe deployment nginx | grep -i image:
```

**Expected output:**

```
Image: nginx:1.17
```

With this command, the deployment rolls back to the previous version.

---

### 🎯 Rollback to a Specific Revision

Rollback to a defined revision using the `--to-revision` flag:

```bash
kubectl rollout undo deployment nginx --to-revision=1
```

**Verify the rollback:**

```bash
kubectl describe deployment nginx | grep -i image:
```

**Expected output:**

```
Image: nginx:1.16
```

**Check the revision history after rollback:**

```bash
kubectl rollout history deployment nginx
```

**Output:**

```
deployment.apps/nginx
REVISION  CHANGE-CAUSE
1         <none>
3         kubectl edit deployments.apps nginx --record=true
4         kubectl set image deployment nginx nginx=nginx:1.17 --record=true
```

---

## ✅ Best Practices

✔️ **Always monitor rollouts** using `kubectl rollout status` to ensure deployments complete successfully

✔️ **Document changes** using annotations or version control (while `--record` is deprecated)

✔️ **Test updates in staging** before rolling out to production

✔️ **Limit revision history** using `revisionHistoryLimit` to control storage

✔️ **Avoid using `latest` tags** — always specify explicit image versions

✔️ **Configure health checks** — implement readiness and liveness probes for safe rollouts

✔️ **Always have a rollback strategy** before deploying to production environments

---

## 📖 Common Commands Reference

| Command                                                    | Description                   |
| ---------------------------------------------------------- | ----------------------------- |
| `kubectl create deployment <name> --image=<image>`         | Create a new deployment       |
| `kubectl rollout status deployment <name>`                 | Monitor rollout progress      |
| `kubectl rollout history deployment <name>`                | View rollout history          |
| `kubectl rollout history deployment <name> --revision=<n>` | View revision details         |
| `kubectl set image deployment <name> <container>=<image>`  | Update container image        |
| `kubectl edit deployment <name>`                           | Edit deployment manifest      |
| `kubectl rollout undo deployment <name>`                   | Rollback last change          |
| `kubectl rollout undo deployment <name> --to-revision=<n>` | Rollback to specific revision |
| `kubectl describe deployment <name>`                       | View deployment details       |

---

## 🛠️ Troubleshooting

### 🚨 Rollout Stuck or Failed

Check pod status and events:

```bash
kubectl get pods -l app=nginx
kubectl describe pod <pod-name>
```

Check deployment events:

```bash
kubectl describe deployment nginx
```

---

### 📦 Image Pull Errors

Verify the image name and version:

```bash
kubectl get deployment nginx -o jsonpath='{.spec.template.spec.containers[0].image}'
```

Check if the image exists in the registry and verify credentials if using a private registry.

---

### ❌ Rollback Not Working

Check the revision history to ensure the target revision exists:

```bash
kubectl rollout history deployment nginx
```

Ensure the target revision exists before attempting rollback. If the revision was pruned due to `revisionHistoryLimit`, it cannot be restored.

---

## 🤝 Contributing

Contributions are welcome!  
Feel free to submit issues or pull requests to improve this guide.

---

## 📄 License

MIT License — free to use for learning and reference.

---

## 👤 Author

Created as a **practical reference for Kubernetes deployment rollout and rollback management**.

---