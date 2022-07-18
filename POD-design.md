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

## Rolling updates & Rollebacks in Deployments
## Job and CronJobs
