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
When you first create a `deployment`, it triggers a rollout. A new rollout creates a new deployment revision. Let's call it `revision 1`.

In the feature when the application is uggraded, meaning when the container version is updated to a new one, a new rollout is triggered and a new deployment revision is created named `revision 2`.

### See status of your rollout
`kubectl rollout status deployment/myapp-deployment`

### Show the revisions and history of our deployment
`kubectl rollout history deployment/myapp-deployment`

### Deployment Strategy
1. `Recreate`: Down all exist contaienr and then create new revision containers.
2. `Rolling update` (default): Down and up container one by one.

### See the detail for rolling update
`kubectl describe deployment myapp-deployment`

### Upgrade deployments
<img width="1610" alt="image" src="https://user-images.githubusercontent.com/6932446/180041503-1325636e-11da-44a5-8e71-dfcd552edca7.png">

`kubectl get replicasets` can see the detail for upgrade deployments.

### Rollout Undo Command
`kubectl rollout undo deployment/myapp-deployment`

### Cheat shhet
#### Create:
`kubectl create -f deployment-definition.yml`

#### Get
`kubectl get deployments`

#### Update
`kubectl apply -f deployment-definition.yml`

`kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1`

#### Status
`kubectl rollout status deployment/myapp-deployment`

`kubectl rollout history deployment/myapp-deployment`

#### Rollback
`kubectl rollout undo deployment/myapp-deployment`

#### creating a deployment and check rollout status
`kubectl create deployment nginx --image=nginx:1.16`

`kubectl rollout status deployment nginx`

`kubectl rollout history deployment nginx`

`kubectl edit deployments nginx --record`

#### Using the --revision flag (You can check sthe status of each revision individually)
`kubectl rollout history deployment nginx --revision=1`

### Using the --record flag (record flag is used to save the commnad used to create/update a deployment)
`kubectl set image deployment nginx nginx=nginx:1.17 --record`

## Job and CronJobs
