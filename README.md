# CKAD Study Notes

Personal notes, cheat sheets, and study plans from my journey toward the **Certified Kubernetes Application Developer (CKAD)** certification.

> **Status:** Passed CKAD with **98/100** 🎉. First attempt was quick and unserious (**55/100**); the retake was the real prep.

These notes are informal and written for my own reference — expect shorthand, TODOs, and scattered reminders rather than polished tutorials. Sharing them in case they help someone else preparing for the same exam.

---

## 📂 What's in this repo

| File / Folder | What it is |
|---|---|
| `CKAD_FINAL_REVIEW.md` | Condensed final-review notes: `kubectl` config/port-forward, events, `top`, Helm, Docker, `kubectl set`, Job/CronJob deadlines, QoS, affinity, multi-container patterns, and other gotchas worth reviewing the night before. |
| `CKAD-1st-quick-attempt.md` | Notes and study schedule from my **first** prep round (LFS158 → LFD259 → CKAD). Contains foundational Kubernetes concepts, imperative command cheats, label selectors, and links to course material. |
| `CKAD-2nd-guaranteed-attempt.md` | Retake plan for **Sept/Oct 2026** plus deeper notes: kube components, security contexts, LimitRange vs ResourceQuota, RBAC, ingress annotations, service resolution, dynamic provisioning, and a list of weak areas to revisit. |
| `Kustomize-cheat-sheet.md` | Kustomize reference: base/overlay model, every key `kustomization.yaml` field, strategic merge patches, and a CKAD checklist. |
| `Manifest-cheat-sheet.md` | Copy-paste YAML patterns for Pods, Deployments, Services, ConfigMaps, Secrets, Volumes/PV/PVC, Probes, Resources, HPA, NetworkPolicy, Jobs, RBAC, Ingress, and more. Also includes a "must know" `kubectl` section. |
| `screenshots/` | Visual quick-reference guides. Includes K8s entities & short forms, common CLI flags, frequent debugging scenarios (TargetPort/Label mismatch), ConfigMap injection methods, Kustomize patch types, and the "Golden Rule" for Services. |

---

## 🗺️ Background

My learning path leading up to CKAD:

1. **LFS158 – Introduction to Kubernetes** (foundation)
2. **LFD259 – Kubernetes for Developers** (developer deep dive)
3. **Mumshad Mannambeth's CKAD course** (exam-focused, hands-on labs)
4. **Hands-on Practice** – Drilling through `jamesbuckett/ckad-questions`, `TiPunchLabs/ckad-dojo`, and the KodeKloud Kubernetes Challenges
5. **Killer.sh** simulator + mock exams
6. **CKAD exam**

The 1st-attempt notes reflect the foundation phase; the 2nd-attempt notes are the retake plan (finish Mumshad → revise → question banks → Killer.sh → exam).

---

## 🔗 Resources I'm using

**Official docs (allowed during the exam)**
- [Kubernetes Glossary](https://kubernetes.io/docs/reference/glossary/?fundamental=true)
- [kubectl Quick Reference](https://kubernetes.io/docs/reference/kubectl/quick-reference/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [CKAD / CKA Exam Tips – Linux Foundation](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)

**Courses**
- [LFS158 – Introduction to Kubernetes](https://trainingportal.linuxfoundation.org/)
- [LFD259 – Kubernetes for Developers](https://trainingportal.linuxfoundation.org/)
- [Mumshad Mannambeth – Certified Kubernetes Application Developer (Udemy)](https://www.udemy.com/course/certified-kubernetes-application-developer/)

**⭐ Practice**
- **[jamesbuckett/ckad-questions](https://github.com/jamesbuckett/ckad-questions)** — a large, well-organized bank of CKAD-style questions with solutions. My go-to for hands-on drilling.
- **[TiPunchLabs/ckad-dojo](https://github.com/TiPunchLabs/ckad-dojo)** — scenario-based practice repo. Great for simulating the "do it in the cluster" feel of the real exam.
- [Killer.sh CKAD Simulator](https://killer.sh/)

**Community write-ups**
- [My CKAD Journey – Hari Overhere (Medium)](https://medium.com/@harioverhere/ckad-certified-kubernetes-application-developer-my-journey-3afb0901014)
- [My CKAD Exam Experience – Atharva Chauthaiwale (LinkedIn)](https://www.linkedin.com/pulse/my-ckad-exam-experience-atharva-chauthaiwale/)
