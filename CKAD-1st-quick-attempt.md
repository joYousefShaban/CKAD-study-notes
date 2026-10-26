Goal:
# Kubernetes Learning Path & Schedule

## Learning Path Overview

1. **Introduction to Kubernetes (LFS158)** → Foundation course
2. **Kubernetes for Developers (LFD259)** → Developer-focused deep dive
3. **Certified Kubernetes Application Developer (CKAD)** → Certification preparation

---

## Course Details & Schedule

### 📘 Introduction to Kubernetes (LFS158)
**18 Reading Chapters**

| Milestone | Target Date |
|-----------|-------------|
| First 10 chapters | March 15 |
| Remaining 8 chapters | March 22 |

### 🎥 Kubernetes for Developers (LFD259)
**9 Video Chapters**

| Milestone | Target Date |
|-----------|-------------|
| First 5 chapters | March 29 |
| Last 4 chapters | April 5 |

### 🏆 Certified Kubernetes Application Developer (CKAD)
| Milestone | Target Date |
|-----------|-------------|
| Exam Preparation | April 12 |
| Exam Preparation | April 19 |
| Exam | Before April 26 |

## 📅 Quick Reference Timeline

| Course | Completion Date |
|--------|-----------------|
| LFS158 | March 22 |
| LFD259 | April 5 |
| CKAD Prep | April 19 |
| CKAD | Before April 26 |

---

## 📘Notes
> **Note on Kubernetes Object Management:**
> - **spec section** → Users define the *desired* state of the object
> - **status section** → Populated by Kubernetes to show the actual *current* state
> - Kubernetes controllers continuously work to make the **status match the spec**

> Important to practice Authentication & authorization
> - ServiceAccount, Role, RoleBinding are essential! (try to recall OAuth feature)

# IMPORTANT!
Don't forget to spend points on copouns! they expire on December 9, 2026!!!!!!!!
https://openprofile.dev/myrewards/coupons
ex: 50% off all training & certification products!!

$ kubectl port-forward svc/web-service 8080:80
$ kubectl expose deployment webserver --name=web-service --type=NodePort
$ kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2 (# For secrets, you have to define the type of secret | usually generic)
$ kubectl exec -ti <POD> -- /bin/sh

$ kubectl debug <POD> --image=busybox (starts a temporary sidecar container (using busybox image) inside the specified Pod for debugging purposes, without restarting the Pod.)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

>- context : A combination of user, cluster name and namespace. A convenient way to switch between combinations of permissions and restrictions. For example you may have a development cluster and a production cluster, or may be part of both the operations and architecture namespaces. This information is referenced from ~/.kube/config.

https://trainingportal.linuxfoundation.org/learn/course/kubernetes-for-developers-lfd259/kubernetes-architecture/kubernetes-architecture?page=14
- Check clusterIP

https://trainingportal.linuxfoundation.org/learn/course/kubernetes-for-developers-lfd259/kubernetes-architecture/kubernetes-architecture?page=15
- API Call flow

****** $ kubectl api-resources
Very important

Jobs:
completions: Total number of successful Pod executions required for the Job to finish.
parallelism: Maximum number of Pods running concurrently.
activeDeadlineSeconds: Maximum execution time for the Job before it is terminated.

Phases to Persistent Storage

https://trainingportal.linuxfoundation.org/learn/course/kubernetes-for-developers-lfd259/deployment-configuration/deployment-configuration?page=7
- Phases to Persistent Storage

Which of the following arguments do we pass to the kubectl rollout command to view object revisions? a. history
What argument do we pass to the kubectl rollout command in order to return to a previous version? c. undo
a. history
b. rollout
c. undo
d. redo

To expose a low port using a Kubernetes object, you would have to use a _________.

a. ClusterIP 
b. NodePort
c. LoadBalancer (Correct answer)
d. Ingress Controller

HPA = more pods.
VPA = bigger/smaller pods.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad?_gl=1%2A1g7ru2t%2A_gcl_au%2AMTg0OTI3MjYxNi4xNzczMzkxMTI2LjE3NDA1OTk2ODcuMTc3NDcwMTYwNC4xNzc0NzAxNjAz%2A_ga%2AODc3NDQ5MS4xNzczMzkxMTI1%2A_ga_EMX7DDZMX4%2AczE3NzU4OTcwMTckbzIxJGcxJHQxNzc1ODk3MDQ3JGozMCRsMCRoMA..#cka-and-ckad-exam-environment
kubectl with k alias and Bash autocompletion
yq for YAML processing
curl and wget for testing web services
man and man pages for further documentation


WORKLOADS STRUCTURE:
apiVersion: apps/v1
kind: Deployment
metadata:
spec:
  template:
    metadata:
    spec:
  selector:
    matchLabels:
  replicas: 1
status:


https://youtu.be/5sAfuG0M-Kg?si=u_kgg7bsudeQRTbw&t=227
- kubectl config current-context
- kubectl get ns
- kubectl log
- curl
- Quick rollback - safety (use kubectl get deploy -o yaml > app.yaml)

https://www.youtube.com/watch?v=jWs1_TfPQoQ
- kubectl get events -n [my-namespace]
- kubectl get logs [POD]
Fix indentation in vim by --> ":set paste" command 

https://www.youtube.com/shorts/YqQz_NJxyE4



Training
https://kubernetes.io/docs/reference/glossary/?fundamental=true
https://kubernetes.io/docs/reference/kubectl/quick-reference/

***** If pod crashed and restarted, get logs about the previous instance through: `kubectl logs nginx -p (or --previous)`

Use -c to connect to a container inside a pod when using sidecars for example. `kubectl logs nginx -c nginx-sidecar -n mynamespace`

To get all pods with a specific label: kubectl get po -L app

## Label Selector Operators

| Operator | Type | Matches | Example |
|----------|------|---------|---------|
| `=`, `==` | Equality | Exact value | `app=nginx` |
| `!=` | Equality | Not equal to value | `app!=nginx` |
| `in` | Set | Any value in list | `"app in (api,web)"` |
| `notin` | Set | Value not in list | `"tier notin (prod,staging)"` |
| `exists` | Set | Label exists (any value) | `tier` |
| `!` | Set | Label doesn't exist | `'!tier'` |
### Quick Rule
- **Single value** → Use `=` or `!=`
- **Multiple values** → Use `in` or `notin`
- **Don't care about value** → Use `exists` or `!`
```
matchLabels:
  app: myapp        # equality-based
matchExpressions:
  - key: env
    operator: In    # set-based
    values: [dev, qa]
  - key: app
    operator: Exists
```

- nodeName to prefer schedule on a specific node based on name
- NodeSelector to prefer schedule on a specifc node based on labels

Taint = Security guard blocking the door
Toleration = Permission slip
nodeSelector = "I prefer this building" — guard doesn't care what you prefer

*   *   *   *   *
│   │   │   │   │
│   │   │   │   └─── Day of week (0-6, 0=Sunday)
│   │   │   └─────── Month (1-12)
│   │   └─────────── Day of month (1-31)
│   └─────────────── Hour (0-23)
└─────────────────── Minute (0-59)

***** kubectl get events --field-selector reason=Failed
*** $ kubectl api-resources
*** $ kubectl expose deploy foo --port=6262 --target-port=8080
* $ kubectl top nodes  (Display CPU/Memory usuage per node or pod)
Very important

    ┌─────────────────────────────────────────────┐
    │                    Node                      │
    │  ┌─────────────────────────────────────┐    │
    │  │         Service (ClusterIP)         │    │
    │  │   listens on:  port: 80             │    │
    │  └─────────────────┬───────────────────┘    │
    │                    │                        │
    │                    ▼                        │
    │  ┌─────────────────────────────────────┐    │
    │  │              Pod                    │    │
    │  │   container app listening on:       │    │
    │  │        targetPort: 8000             │    │
    │  └─────────────────────────────────────┘    │
    │                                            │
    │   NodePort: 31070  ──►  forwards to port 80│
    └─────────────────────────────────────────────┘
           ▲
           │
    [curl nodeIP:31070]

For hostPath volumes, you have to make sure both pods are on the same node, otherwise files might be missing! (not the case for networking storages)

kubectl cp busybox:/etc/passwd ./passwd # Copy files from pod to local


helm search (find hub or repo)
* helm repo add <URL>
** helm install -f values.yaml mydeploy .
** helm install mydeploy bitnami/nginx --set replicaCount=5
** helm uninstall mydeploy
** helm list
** helm status
** helm history mydeploy
** helm upgrade -f values.yaml -f overwrite.yaml mydeploy .
** helm rollback mydeploy

--dryrun
--debug (prints kubernetes manifest)

----
# CKAD Essential Commands Cheatsheet

## Workloads
kubectl create deployment <name> --image=93
kubectl run <pod> --image=93                    # Single pod
kubectl apply -f <file>
kubectl edit deployment/<name>
kubectl scale deployment/<name> --replicas=<n>

## Rollouts
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout restart deployment/<name>

## Debugging
kubectl get pods -o wide
kubectl describe pod/<name>
kubectl logs <pod> [-c <container>]
kubectl logs <pod> --previous                    # Crash loop
kubectl exec -it <pod> -- sh
kubectl port-forward pod/<name> 8080:80
kubectl top 

## CleanUp
kubectl delete deployment/<name>
kubectl delete -f <file>

## Docker commands
### 1. Build an image from a Dockerfile
docker build -t myapp:1.0 .

### 2. Save an image to a tar file (backup/share)
docker save -o myapp.tar myapp:1.0

### 3. Load an image from a tar file
docker load -i myapp.tar

### 4. List images on the host
docker images

### 5. Remove an image
docker rmi myapp:1.0

### 6. Run a container to test your image
docker run -it myapp:1.0 sh

### Unzip .tar file
tar -xf file.tar