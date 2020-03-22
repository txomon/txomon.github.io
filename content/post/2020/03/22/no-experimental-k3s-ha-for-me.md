---
title: "No Experimental K3S HA for Me"
date: 2020-03-22T16:27:18+01:00
---

So after hitting my head a while against the HA setup with dqlite that k3s support in "Experimental" phase, had to let go after hitting https://github.com/rancher/k3s/issues/1391

Good thing is that with only one master, things are way faster.

I have also deployed a DaemonSet with DigitalOcean Agent on it so that I can have some metrics that don't rely on k3s.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: do-agent
spec:
  selector:
    matchLabels:
      app: do-agent
  template:
    metadata:
      name: do-agent
      labels:
        app: do-agent
    spec:
      containers:
        - name: do-agent
          image: digitalocean/do-agent:stable
          imagePullPolicy: Always
          volumeMounts:
            - mountPath: /host/proc
              name: host-proc
            - mountPath: /host/sys
              name: host-sys
      volumes:
        - name: host-sys
          hostPath:
            path: /sys
        - name: host-proc
          hostPath:
            path: /proc
```

BTW, the kubernetes plugin for IntelliJ based IDEs works like a charm.
