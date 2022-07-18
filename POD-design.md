# POD Design
## Labels, Selectors and Annotations
### Label your node
``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-app
  labels:
    app: App1
    function: Front-end
```
### select your pod
``` kubectl get pod --selector app=App1 ```
### ReplicaSet
``` yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  # Label in replicaset
  labels:
    app: App1
    function: Front-end
spec:
  replicas: 3
  selector:
    # match the label on pod
    matchLabels:
      app: App1
  template:
    metadata:
      # Label in pod
      labels:
        app: App1
        function: Fronte-end
    spec:
      containers:
       ...
```
### Annotations
``` yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  labels:
    app: App1
    function: Front-end
  annotations:
    buildversion: 1.34
    # other detail information
```

## Rolling updates & Rollebacks in Deployments
## Job and CronJobs
