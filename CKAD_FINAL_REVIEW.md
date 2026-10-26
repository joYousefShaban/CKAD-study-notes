Review:

> kubectl config
> kubectl port-forward
> kuebctl get events
> kubectl get events --field-selector reason=Failed
> kubectl top
> helm
> docker
> kubectl set

Training
https://kubernetes.io/docs/reference/glossary/?fundamental=true
https://kubernetes.io/docs/reference/kubectl/quick-reference/

> kubectl annotate po nginx{1..3} description- owner-
# nginx{1..3} itrates through nginx1 nginx2 nginx3
# description- owner- removes the annotations defined there

activeDeadlineSeconds → max time a Job can run.
startingDeadlineSeconds → how late a CronJob can start after its scheduled time; if later, that run is skipped.

demo@DESKTOP-6VQ2RPL:/mnt/d/Desktop/PProjects/DevOps Projects/CKAD Preps/ckad-dojo/ckad-dojo-my-take$ k explain cj.spec | grep -i deadline
  startingDeadlineSeconds       <integer>
    Optional deadline in seconds for starting the job if it misses scheduled
demo@DESKTOP-6VQ2RPL:/mnt/d/Desktop/PProjects/DevOps Projects/CKAD Preps/ckad-dojo/ckad-dojo-my-take$ k explain job.spec | grep -i deadline
  activeDeadlineSeconds <integer>
    resetting the ActiveDeadlineSeconds timer too. Defaults to false.

kind: Pod
spec:
  containers:
  - name: lifecycle-demo-container
    image: nginx
    lifecycle:
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo Hello from the postStart handler > /usr/share/message"]
      preStop:
        exec:
          command: ["/bin/sh","-c","nginx -s quit; while killall -0 nginx; do sleep 1; done"]

Kubernetes QoS controls which Pods get evicted first during resource shortages.
• Guaranteed: requests == limits (Safe)
• Burstable: requests < limits (Medium risk)
• BestEffort: No requests/limits (Killed first)

kind: Service
spec:
  ports:
  - name: http
    port: 80
    targetPort: http-web # instead of "80" here
    protocol: TCP
#NOTE: Named ports allow services to reference ports by name rather than number. This makes configuration more readable and allows pods to change their port numbers without updating service definitions.

"with / co-locate / same node as" → podAffinity.
"away / spread / not on same node" → podAntiAffinity.

Multi-containers:
- Prompt says “sidecar” → put it in initContainers with restartPolicy: Always.
- Prompt says “init/setup task” → put it in initContainers with no restartPolicy (classic init).
- Prompt says “extra/co-located container” or nothing about sidecar → put it in containers.

> k set image -h | grep -i record
Doesn't show --record flag, but you can still use it!

>port-forward 8080:80 → local 8080 → pod 80
>--tcp=80:8080 → service 80 → container 8080
>docker -p 8080:80 → host 8080 → container 80
Note: : Left = what you connect to. Right = where it goes.

Scheduling:
- Taints/Tolerations → keep pods out
- nodeSelector → simple target
- Affinity → flexible/complex target or pod relationships
- nodeName → bypass scheduler

nodeAffinity → target Nodes by label. Use when: "run on nodes with SSD/GPU/zone."
podAffinity → target other Pods by label. Use when: "run next to this pod" (same node/zone).
Rule: nodeAffinity = which node, podAffinity = which pod nearby.

IMPORTANT!
> k get secret => show encoded value
> k describe secret => shows decoded value (no need to pipe it again into a base64)

> overwrites (replaces all content).
>> appends (adds to the bottom).
| (pipe) sends the output of the left command into the right command.

