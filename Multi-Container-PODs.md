# Multi-Container PODs

## Multi-Container PODs
## Init Containers
Init containers is run only once before the POD is initialize.
``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ;']
```
If any of the initContainers fail to complete, Kubernetes restarts the Pod repeatedly until the Init Container succeeds.
