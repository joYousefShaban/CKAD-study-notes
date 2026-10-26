# 🎯 CKAD Retake Plan — September/October 2026

**Study:** Weekends only  
**Strategy:** Finish Mumshad in order → Revise → Practice → Killer.sh → Exam  
**Target Exam:** October 17–18, 2026

| Weekend | Focus Areas | Must-Do Actions |
|---|---|---|
| **W1 – Sep 5–6** | 🚀 **Mumshad: Sections 0–78** – ReplicaSets → Services → Imperative Commands → Images → ConfigMaps → Secrets → Security → Scheduling | Complete course sections **22–78** in order + do the included labs |
| **W2 – Sep 12–13** | 🔧 **Mumshad: Sections 80–137** – Multi-container Pods → Init Containers → Deployments → Jobs/CronJobs → Network Policies → Ingress → Storage → StatefulSets | Complete course sections **80–137** in order + do the included labs |
| **W3 – Sep 19–20** | 🔐 **Mumshad: Sections 138–195** – Security → RBAC → API → CRDs → Helm → Kustomize | Complete course sections **138–195** in order + do the included labs |
| **W4 – Sep 26–27** | ⚡ **Mumshad: Sections 196–213** – Challenges → Time Management → Lightning Labs → Mock Exams | Finish the course + complete Lightning Labs + Mock Exams 1 & 2 |
| **W5 – Oct 3–4** | 🔄 **Revision + Question Banks** – LFS158, LFD259 and previous CKAD preparation | Revise old courses/notes + intensive CKAD question banks + record weak areas |
| **W6 – Oct 10–11** | 🔥 **Killer.sh + Targeted Practice** – Full exam simulation | Killer.sh Session 1 + analyse mistakes + targeted practice + revisit weak topics |
| **W7 – Oct 17–18** | 🏆 **Final Polish + CKAD Exam** – Final revision and exam readiness | Re-do failed questions + Killer.sh review + `kubectl explain` + official docs + PSI check → **CKAD Exam** |

## 🎯 Readiness Target

**Previous score:** 55/100  
**Passing score:** 66/100  
**Target:** 75–85+ consistently in practice

> **Main principle:** Follow the Mumshad course in its original order. Since you already know the concepts, move quickly through familiar material and spend your time on labs and hands-on practice.

## 📚 Study Priority

1. 🧪 **Hands-on labs**
2. 📝 **CKAD question banks**
3. 🔥 **Killer.sh**
4. 🎯 **Mock exams**
5. 🔄 **LFS158/LFD259 revision**
6. 📖 **Video refreshers**

> **Rule:** Don't stop to deeply study familiar concepts. Keep moving through the course and slow down only when you encounter a weak area.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Node is a working machine
Cluster is a collection of nodes
Master is a node that manages other nodes within cluster


Kube compoenents:
- API Server => front end for kubernetes, all comunications go through APIServer
- etcd => key-value pair to store all data
- scheduler => creating containers and assigning them
- controller => noticing/responding to containers/nodes going down (aka. the brain behind kube)
- container runtime => underlying software to run containers (eg. docker)
- kubelet => agent that run on each node on each cluster, ensure containers are running as expected

Required fields:
- apiVersion => version of KubernetesAPI you are using to create the object
  - v1 => pod & services | apps/v1 => replicaSet & deployment
- kind
- metadata
- spec (for Pods & Workloads) - data (for Configmaps & Secrets)

> kubectl get all #Command to get all objects

34. Certification Tip: Imperative Commands => https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/14112621#notes

> kubectl api-resources #list all resources supported by API

36. Kubectl Explain Command => https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/52883739#notes
  > kubectl explain pod --recursive #List all possible configurations of pod resource

> Docker history #Check layers
> Docker inspect #Check general info

| Dockerfile Directive | Kubernetes Field                     |
| :------------------- | :----------------------------------- |
| `ENTRYPOINT`         | `spec.containers[].command`          |
| `CMD`                | `spec.containers[].args`             |
If you don't get it, check: https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/12308344#overview

> docker run --user=1001 ubuntu #Set user
> docker run --cap-add MAC_ADMIN ubuntu #Set capabilities

kind: Pod
spec:
  securityContext:
    runAsUser: 1000 # pod level configuration of user
  containers:
  - name: ubuntu
    image: ubuntu
    securityContext:
      runAsUser: 1000 # container level configuration of user
      capabilities: # container level configuration of capabilities. (IMPORTANT: can't be set on pod level)
        add: ["MAC_ADMIN"]

kind: LimitRange #INDIVISUAL POD LEVEL - Defaults resources or enforce a min/max range of definitions
kine: ResourceQuota #NAMESPACE LEVEL - enforce a total request/limit by all pods

VIM:
X>>
Indent a tab to X amount of lines

Taints(node)+Tolerations(pod) allow a Pod to go to a node (but don't force it).
Node Selectors force the Pod to go only to that specific node (guaranteeing placement).

afaik: use affinity for custom more complex filtering, or pinning specific node

Iteresting settings for VIM:
set expandtab
set shiftwidth=2
set tabstop=2
set nu

Check:
- https://medium.com/@harioverhere/ckad-certified-kubernetes-application-developer-my-journey-3afb0901014
- https://www.linkedin.com/pulse/my-ckad-exam-experience-atharva-chauthaiwale/

> kubectl top nodes #Get resource consumption in nodes
> kubectl top pods  #Get resource consumption in pods

> kuebctl get [resource] --selector labelKey=labelValue #Filter based on label

Labels => Group & select objects
Annotations => record informatory details (ex: name, build version, maintainers)

https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/24491680#overview (7:30+) VERY VERY IMPORTANT

Ingress annotations (NGINX):
May appear: Yes, annotations like rewrite-target and ssl-redirect.
When to use rewrite-target: Only if the backend serves at root (/) but the Ingress path has a prefix (e.g., /watch). The exam tells you this; you need to read question well as you cannot see it via kubectl get.
Syntax: nginx.ingress.kubernetes.io/rewrite-target: /
Where: metadata.annotations
API: networking.k8s.io/v1.
Allowed docs: You can access NGINX Ingress docs during the exam.

helm rollback [RELEASE] [REVISION]
helm upgrade [RELEASE] [CHART]

Use Strategic merge patch for kustomize patches, as it's easier. (just include apiVersion, Kinda, metadata. -- spec will be the merge concenpt.)
For delete of lists, since you cannot define it as array, only solution would be:
- name: xyz (name of list entry)
  $patch: delete (delete op)


Free exam/challanges:
https://learn.kodekloud.com/learn/courses/kubernetes-challenges

https://www.youtube.com/watch?v=rnemKrveZks
> kubectl set serviceaccount deployment <deploy-name> <sa-name>

> k get pods | wc -l
Count lines of occurences (make sure to substract 1 from total to remove header. or add `--remove-headers` flag)

https://www.youtube.com/watch?v=ggSyF1SVFr4
> :%s/<search_string>/<replace_string>/gc
=> g for all references
=> c for confirmation text

Bash terminal: Alt+B (back) | Alt+F (forward)
Vim: Esc, then b (back) | w (forward) | e (end of next word)

Kubernetes Service Resolution:
Different Namespace: <service-name>.<target-namespace>.svc.cluster.local
Same Namespace: <service-name>

NOT MENTIONED!!!! Configmap/Secrets general page don't mention "envFrom:"
This is important to mount all keys. Example:
spec:
  containers:
    - name: test-container
      image: registry.k8s.io/busybox:1.27.2
      command: [ "/bin/sh", "-c", "env" ]
      envFrom:
      - configMapRef: # secretRef:
          name: special-config #secretName:

NOT MENTIONED!!!! SecurityContext settings are hard to find in docs.
*IMPORTANT*: SecurityContext.capabilities are only allwoed in container level, NOT pod level!
pod.spec.containers.capabilities.drop: ["ALL"] # drop all capabilities

> k top node #Gets node metrics report of mem/cpu usage (if metrics API installed)
> k top pods #Gets pods metrics report of mem/cpu usage (if metrics API installed)

*TODO*: learn how to search based on label key or value!

*TODO*: re-check endpoints & learn them.

If a resource doesn't exist, you'll get this output:
> k get ingress -o yaml
apiVersion: v1
items: []
kind: List
metadata:
  resourceVersion: ""

*TODO*: redo ingress labs

Dynamic volume provisioning means Kubernetes automatically creates a PersistentVolume (PV) when you create a PersistentVolumeClaim (PVC), using a StorageClass and its provisioner.
*Static*: You manually create the PV first.
*Dynamic*: Kubernetes creates the storage volume for you on demand.

*TODO*: docker labs.
*IMPORTANT*: YOU MUST DEFINE flags/options before iamge `Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`. Otherwise they'll be considered as ARGs, and you'll keep getting errors. Example:
> docker run -p 8282:8080 webapp
-- in -p command, 8282 is hostPort & 8080 is containerPort

> kubectl config set-context #Creates or edits a context in your kubeconfig. It does not switch to it.
> kubectl config use-context #Switches kubectl to an existing context. It changes your active context.
NOTE: append `--kubeconfig=/root/my-kube-config` flag for referencing another kconfig file

RBAC rules MUST BE in plurar!
#kind: Role
#metadata:
  #name: developer
#rules:
#- apiGroups:
  #- apps
  #resources:
  #- deployment # THIS IS WRONG. (deploy/deployment) X (deployments) -/
  #verbs:
  #- get

`/etc/*release*` to find os info

*TODO*: redo Kustomize labs

`pod.spec.env` looks like:
```
#env:
  #- name: MYSQL_ROOT_PASSWORD
    #value: mypassword
and NOT:
#env:
  #- name: MYSQL_ROOT_PASSWORD.mypassword
```

`pod.spec.nodeName`= controlPlane -- this will hard-code node based on name. SUPER helpful
`pod.spec.nodeSelector:` <label-key>:<label-value> -- schedule based on label

DoubleCheck: Different types of services
DoubleCheck: Different types of workloads (deploy, stateful, deamon)
DoubleCheck: Different types of probe definitions (http, TCP, exec)

> kubectl logs
> kubectl logs -c          # multi-container pod
> kubectl logs --previous  # crashed container

docker build -t my-app:1.0 .
docker save -o my-app.tar my-app:1.0 