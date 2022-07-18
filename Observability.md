# Observability
## `Readiness Probe` & `Liveness Probe`
> `Readiness Probe` is used to check container is in ready status. `Liveness Probe` is used to detect the live status on a ready container. and control the restart action
* POD Status
  * `Pending`
  * `ContainerCreating`
  * `Running`
* POD Conditions (Every condition will be true/false status)
  * `PodScheduled`: When a POD is scheduled on a node
  * `Initialized`: When a POD is initialized
  * `ContainersReady`: When a POD has multiple containers, and when all the containers in the POD are ready.
  * `Ready`: The POD itself is considered to be ready.
* Look for POD conditions
  `kubectl describe pod {xxx}` and look for Condictions section
* How kubenetes know that the application inside a container is really ready status or not? `Readiness Probe`
* How container cehck the ready status?
  * HTTP test: /api/ready
  * TCP test: 3006
  * Exec Command:
* How to config `Readiness Probe` in POD config?
  * Http test
    ``` yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: simple-webapp
      label:
        name: simple-webapp
    spec:
      containers:
      - name: simple-app
        image: simple-app
        ports:
          - containerPort:8080
        readinessProbe:
          httpGet:
            path: /api/ready
            port: 8080
    ```
  * TCP test
    ``` yaml
    readinessProbe:
      tcpSocket:
        port:3306
    ```
  * Exec command
    ``` yaml
    readinessProbe:
      exec:
        command:
          - cat
          - /app/is_ready
      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 8
    ```
 ## Liveness Probe
 The same as `readiness probe`
 
 ## Contianer Logging
 ### Docker logging
 ``` docker logs -f [contaien_id] ```
 ### Pod logging
 ``` kubectl logs -f [pod_name] ```
 ### Multiple container in pod
 ``` kubectl logs -f [pod_name] [container_name] ```
 
 ## Monitoring Kubernetes
 ### OSS monitor solution
 * METRICS SERVER
 * Prometheus
 * Elastic Stack
 * DATADOG
 * Dynatrace
 ### METRICS SERVER
 * kubelet sub component - cAdvisor
 * install metrics-server
   * minikube
     * ```minikube addons enable metrics-server```
   * other 
     * ``` git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git```
     * ``` kubectl create -f . ``` 
 * list top node: `kubectl top node`
 * list top pod: `kubectl top pod`
