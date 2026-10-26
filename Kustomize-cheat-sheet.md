# Kustomize Cheat Sheet for CKAD

## 1. What is Kustomize?

**Kustomize** is a Kubernetes-native configuration customization tool.

- It customizes YAML **without templates**.
- It uses a **base + overlay** model.
- It is built into `kubectl`.
- No separate installation is required for CKAD.

**Core idea:** Keep common YAML in a **base**, then apply small environment-specific changes in **overlays**.

---

## 2. Base and Overlay

### Base
A **base** is a directory containing common Kubernetes YAML resources and a `kustomization.yaml`.

It defines the shared/default version of resources.

### Overlay
An **overlay** is a directory that references a base and applies environment-specific changes.

Examples: `dev`, `staging`, `prod`.

### Directory Structure

```text
app/
├── base/
│   ├── kustomization.yaml
│   ├── deployment.yaml
│   └── service.yaml
└── overlays/
    ├── dev/
    │   └── kustomization.yaml
    └── prod/
        └── kustomization.yaml
```

---

## 3. `kustomization.yaml` Key Fields

| Field | Definition |
| :--- | :--- |
| `resources` | List of YAML files or directories to include in the build. |
| `namespace` | Sets the namespace for all generated resources. |
| `namePrefix` | Adds a prefix to all resource names. |
| `nameSuffix` | Adds a suffix to all resource names. |
| `commonLabels` | Adds labels to all resources and selectors. |
| `commonAnnotations` | Adds annotations to all resources. |
| `images` | Overrides container image names or tags. |
| `replicas` | Changes replica count for workloads. |
| `configMapGenerator` | Generates ConfigMaps from literals or files. |
| `secretGenerator` | Generates Secrets from literals or files. |
| `patches` | Applies patches to modify base resources. |

---

## 4. Example `kustomization.yaml` Covering All Key Fields

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# resources: YAML files or directories to include
resources:
- deployment.yaml
- service.yaml

# namespace: sets namespace for all resources
namespace: staging

# namePrefix / nameSuffix: modify all resource names
namePrefix: dev-
nameSuffix: -v1

# commonLabels: added to all resources and selectors
commonLabels:
  app.kubernetes.io/part-of: my-app
  environment: dev

# commonAnnotations: added to all resources
commonAnnotations:
  owner: platform-team
  contact: team@example.com

# images: override image name/tag
images:
- name: nginx
  newName: my-registry/nginx
  newTag: "1.25.3"

# replicas: change replica count for a workload
replicas:
- name: my-app
  count: 3

# configMapGenerator: generate ConfigMaps from literals/files
configMapGenerator:
- name: app-config
  literals:
  - APP_ENV=dev
  - LOG_LEVEL=debug
  files:
  - application.properties

# secretGenerator: generate Secrets from literals/files
secretGenerator:
- name: app-secret
  literals:
  - DB_USER=admin
  - DB_PASSWORD=change-me
  files:
  - tls.crt
  type: Opaque

# patches: apply partial YAML changes to base resources
patches:
- path: increase-resources.yaml
  patch: |-
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-app
    spec:
      template:
        spec:
          containers:
          - name: my-app
            resources:
              requests:
                cpu: 500m
                memory: 512Mi
```

**Generate this file quickly:** Use `kustomize create` instead of writing the header manually.  
- `--autodetect` searches the current directory for Kubernetes YAML files and adds them to `resources`.  
- `--recursive` extends that search into all subdirectories.

```bash
kustomize create --autodetect --recursive --namespace=myapp
```

---

## 5. Patching

### Patch
A **patch** is a partial YAML definition used to modify specific fields of a base resource.

The most common exam type is a **strategic merge patch**.

- It matches resources by `kind` and `metadata.name`.
- Only specified fields are changed.
- All unspecified fields remain unchanged.

### Example Overlay `kustomization.yaml`

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
- ../../base
patches:
- path: increase-resources.yaml
```

### Example Patch File

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
spec:
  template:
    spec:
      containers:
      - name: api
        resources:
          requests:
            cpu: 500m
            memory: 512Mi
```

---

## 6. CKAD Quick Checklist

- [ ] Know that Kustomize is template-free and built into `kubectl`.
- [ ] Understand the base + overlay pattern.
- [ ] Memorize key `kustomization.yaml` fields.
- [ ] Be able to write a strategic merge patch.
- [ ] Practice `kubectl kustomize` and `kubectl apply -k`.
- [ ] Know imperative `kustomize edit set ...` commands.