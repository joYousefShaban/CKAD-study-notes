# 🧠 CKAD Cheatsheet

> 🎯 Memorize the patterns. Look up obscure fields.

## 🟦 Pod — Complete Common Pattern

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
  labels:
    app: app

spec:
  # 🛡️ Pod-level security
  securityContext:
    runAsUser: 1000
    runAsGroup: 1000
    runAsNonRoot: true
    fsGroup: 2000

  # 🔄 Init containers
  initContainers:
  - name: init
    image: busybox
    command: ["sh", "-c", "echo init"]

  # 📦 Containers
  containers:
  - name: app
    image: nginx:latest

    # 🌐 Ports
    ports:
    - containerPort: 80

    # ⚙️ Environment
    env:
    - name: ENV
      value: "prod"
    - name: CONFIG
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: CONFIG

    # 🔐 Secret / ConfigMap
    envFrom:
    - configMapRef:
        name: app-config
    - secretRef:
        name: app-secret

    # 💰 Resources
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 500m
        memory: 256Mi

    # ❤️ Probes
    livenessProbe:
      httpGet:
        path: /health
        port: 80
      initialDelaySeconds: 10
      periodSeconds: 10

    readinessProbe:
      httpGet:
        path: /ready
        port: 80
      periodSeconds: 5

    # 💾 Mount volume
    volumeMounts:
    - name: data
      mountPath: /data

    # 🔒 Container security
    securityContext:
      runAsNonRoot: true
      allowPrivilegeEscalation: false

  # 💾 Pod volumes
  volumes:
  - name: data
    emptyDir: {}

  # 🔐 ConfigMap volume example:
  # - name: config
  #   configMap:
  #     name: app-config

  # 🔑 Secret volume example:
  # - name: secret
  #   secret:
  #     secretName: app-secret

  # 📂 HostPath volume example:
  # - name: host-data
  #   hostPath:
  #     path: /var/data
  #     type: Directory

  # 💿 PVC volume example:
  # - name: data
  #   persistentVolumeClaim:
  #     claimName: app-pvc

  # 🖥️ Scheduling
  nodeSelector:
    disktype: ssd

  tolerations:
  - key: dedicated
    operator: Equal
    value: gpu
    effect: NoSchedule

  # 🔄 Restart policy
  restartPolicy: Always
~~~

### 🧠 Pod fields worth memorizing

~~~text
spec
├── securityContext
├── initContainers[]
├── containers[]
│   ├── image
│   ├── ports[]
│   ├── env[]
│   ├── envFrom[]
│   ├── resources
│   ├── livenessProbe
│   ├── readinessProbe
│   ├── volumeMounts[]
│   └── securityContext
├── volumes[]
│   ├── emptyDir
│   ├── configMap
│   ├── secret
│   ├── hostPath
│   └── persistentVolumeClaim
├── nodeSelector
├── tolerations[]
└── restartPolicy
~~~

## 🟦 Pod

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  containers:
  - name: app
    image: nginx
    ports:
    - containerPort: 80
~~~

## 🚀 Deployment

~~~yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
      - name: app
        image: nginx
~~~

🧠 `selector == template.labels`

## 🌐 Service

~~~yaml
apiVersion: v1
kind: Service
spec:
  selector:
    app: app
  ports:
  - port: 80
    targetPort: 8080
    # nodePort: 30080
~~~

🧠 `port` → Service  
🧠 `targetPort` → Container  
🧠 `nodePort` → Node

## 🏷️ Label Selectors

**CLI**

~~~bash
kubectl get pods -l app=nginx              # equality
kubectl get pods -l app!=nginx             # not equal
kubectl get pods -l 'env in (dev,qa)'      # set
kubectl get pods -l 'env notin (dev,qa)'   # not in set
kubectl get pods -l app                    # exists
kubectl get pods -l '!app'                 # does not exist
~~~

**YAML**

~~~yaml
selector:
  matchLabels:
    app: nginx          # equality
  matchExpressions:
  - key: env
    operator: In
    values: [dev, qa]
  - key: env
    operator: NotIn
    values: [dev, qa]
  - key: app
    operator: Exists
  - key: app
    operator: DoesNotExist
~~~

🧠 `matchLabels` = equality  
🧠 `matchExpressions` = set-based  
🧠 Service `spec.selector` only supports plain map (`app: nginx`)

## ⚙️ ConfigMap

~~~bash
kubectl create configmap app --from-literal=KEY=value
~~~

~~~yaml
env:
- name: KEY
  valueFrom:
    configMapKeyRef:
      name: app
      key: KEY
~~~

💡 All keys:

~~~yaml
envFrom:
- configMapRef:
    name: app
~~~

## 🔐 Secret

~~~bash
kubectl create secret generic app --from-literal=KEY=value
~~~

~~~yaml
env:
- name: KEY
  valueFrom:
    secretKeyRef:
      name: app
      key: KEY
~~~

💡 All keys:

~~~yaml
envFrom:
- secretRef:
    name: app
~~~

## 💾 Volumes

~~~yaml
volumeMounts:
- name: data
  mountPath: /data

volumes:
- name: data
  emptyDir: {}
~~~

### PVC

~~~yaml
volumeMounts:
- name: data
  mountPath: /data

volumes:
- name: data
  persistentVolumeClaim:
    claimName: app-pvc
~~~

🧠 `volumeMounts` → Container  
🧠 `volumes` → Pod

## 💿 PVC

~~~yaml
apiVersion: v1
kind: PersistentVolumeClaim
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
~~~

## 💿 PV

~~~yaml
apiVersion: v1
kind: PersistentVolume
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /data
~~~

🧠 `PV` = storage  
🧠 `PVC` = request

## ❤️ Probes

~~~yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
~~~

💡 Also:

~~~yaml
livenessProbe:
  tcpSocket:
    port: 8080
~~~

~~~yaml
readinessProbe:
  exec:
    command: ["cat", "/tmp/healthy"]
~~~

🧠 Liveness → restart?  
🧠 Readiness → receive traffic?  
🧠 Startup → started?

## 📦 Resources

~~~yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 500m
    memory: 256Mi
~~~

🧠 `requests` → scheduling  
🧠 `limits` → maximum

## 📏 LimitRange

~~~yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: limits
spec:
  limits:
  - default:
      cpu: 500m
      memory: 256Mi
    defaultRequest:
      cpu: 100m
      memory: 128Mi
    type: Container
~~~

~~~bash
kubectl get limitrange
kubectl describe limitrange limits
~~~

🧠 Sets default requests/limits per Container in a namespace

## 🧮 ResourceQuota

~~~yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: quota
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
    pods: "10"
~~~

~~~bash
kubectl get resourcequota
kubectl describe resourcequota quota
~~~

🧠 Caps total resource usage in a namespace

## 📈 Horizontal Pod Autoscaler

~~~yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
~~~

~~~bash
kubectl autoscale deployment app --min=1 --max=10 --cpu-percent=50

kubectl get hpa
kubectl describe hpa app-hpa
~~~

🧠 Scales Pods based on metrics (CPU/memory/custom)  
🧠 Requires metrics-server

## 🚧 NetworkPolicy

~~~yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
~~~

🧠 `podSelector` → affected Pods  
🧠 `from` → allowed source  
🧠 `ports` → allowed traffic

## 🏃 Job

~~~yaml
apiVersion: batch/v1
kind: Job
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: job
        image: busybox
        command: ["sh", "-c", "echo hello"]
~~~

## ⏰ CronJob

~~~yaml
apiVersion: batch/v1
kind: CronJob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: Never
          containers:
          - name: job
            image: busybox
~~~

## 🔄 Init Container

~~~yaml
spec:
  initContainers:
  - name: init
    image: busybox
    command: ["sh", "-c", "echo init"]
  containers:
  - name: app
    image: nginx
~~~

🧠 `initContainers` → run first

## 🔒 SecurityContext

~~~yaml
spec:
  securityContext:
    runAsUser: 1000
    runAsNonRoot: true
    fsGroup: 2000
~~~

## 👤 ServiceAccount

~~~yaml
spec:
  serviceAccountName: app-sa
~~~

~~~bash
kubectl create serviceaccount app-sa
~~~

## 🛡️ RBAC

### Role

~~~yaml
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
~~~

### RoleBinding

~~~yaml
subjects:
- kind: ServiceAccount
  name: app-sa

roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
~~~

🧠 Role → WHAT  
🧠 RoleBinding → WHO

## 🖥️ Scheduling

### nodeSelector

~~~yaml
nodeSelector:
  disktype: ssd
~~~

### Toleration

~~~yaml
tolerations:
- key: dedicated
  operator: Equal
  value: gpu
  effect: NoSchedule
~~~

🧠 Taint 🚫 repels  
🧠 Toleration ✅ allows

## 🌍 Ingress

~~~yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
spec:
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app
            port:
              number: 80
~~~

# ⚡ kubectl — MUST KNOW

## 🔍 Inspect

~~~bash
kubectl get po
kubectl get po -o wide
kubectl get all
kubectl describe po POD
kubectl logs POD
kubectl logs POD -c CONTAINER
kubectl exec -it POD -- sh
~~~

## 🛠️ Manage

~~~bash
kubectl apply -f file.yaml
kubectl delete -f file.yaml

kubectl scale deploy APP --replicas=3

kubectl rollout status deploy APP
kubectl rollout undo deploy APP
~~~

## 💾 Storage / Network

~~~bash
kubectl get pv,pvc
kubectl get svc
kubectl get nodes
~~~

## 🐛 Debug

~~~bash
kubectl get events --sort-by=.metadata.creationTimestamp

kubectl run tmp --rm -it --image=busybox -- sh
~~~

## 🏗️ Generate YAML

~~~bash
kubectl run nginx --image=nginx --dry-run=client -o yaml

kubectl create deployment app \
  --image=nginx \
  --dry-run=client -o yaml
~~~

## 📖 Forgot a Field?

~~~bash
kubectl explain pod.spec.containers
kubectl explain pod.spec.containers.livenessProbe
~~~

# ⭐ MUST MEMORIZE

~~~text
🚀 Deployment
   selector == template.labels

🌐 Service
   selector → Pods
   port → targetPort

🏷️ Label Selectors
   matchLabels → equality
   matchExpressions → set-based
   CLI: -l app=nginx / 'env in (dev,qa)' / '!app'

💾 Volume
   volumeMounts → Container
   volumes → Pod

💿 Storage
   PV → storage
   PVC → request

❤️ Probes
   httpGet / tcpSocket / exec
   liveness / readiness / startup

📦 Resources
   requests / limits

📏 LimitRange
   default requests/limits per Container

🧮 ResourceQuota
   total namespace limits

📈 HPA
   scales Pods via metrics (CPU/memory/custom)
   requires metrics-server

🚧 NetworkPolicy
   podSelector / from / ports

🛡️ RBAC
   Role → WHAT
   Binding → WHO

🔄 Init
   initContainers → first

🖥️ Scheduling
   nodeSelector / tolerations

🐛 Debug
   get → describe → logs → exec

📖 Forgotten field
   kubectl explain
~~~