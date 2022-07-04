# Node Scheduling
## Taints and Tolerations
* Taints and Tolerations Purpose:
  * 設計如何讓pod不要分配到某個worker node。
  * **Taints** is for node, and **Tolerations** is for pod.
* How to assign **taint** to a node?
  ```
  kubectl taint nodes [node_name] [key]=[value]:[effect]
  ```
* How to remove **taint** from a node?
	```
	kubectl taint nodes [node_name] [key]:[effect]-
  ```
* What is all possible `[effect]`?
	* *NoSchedule*: 對於新的pod不要assign進此node，不影響node上既有pod。
	* *PerferNoSchedule*: K8S盡量不要assign pod到此node，但是最後沒辦法的話才用此node，不影響node上既有pod。 
	* *NoExecute*: 對於新的pod不要assign進此node，並且會停止node上既有的pod。
* How to set `tolerations` to a pod?
  * case 1
  ``` yaml
  # 表示接受帶有key=value & effect=NoScuedle的taint
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
	```
  * case 2
  ``` yaml
  # 表示接受key存在(不管value多少) & effect=NoScuedule的taint
  tolerations:
  - key: "key"
    operator: "Exists"
    effect: "NoSchedule"
  ```
  * case 3
  ``` yaml
  # 若僅有設定 operator 為 Exists，卻沒有設定 key，那表示會 tolerate(容忍) 所有的 taint
  tolerations:
  - operator: "Exists"
  ```
  * case 4
  ``` yaml
  # 僅有設定 key，沒有設定 effect，表示只有帶有相同 key 的 taint 都會被容忍
  tolerations:
  - key: "key"
    operator: "Exists"
  ```
  * opeartor default value is `Equal`
* Query one node's taint status
	```
	kubectl describe nodes [node_name] | grep Taint
	```
## Node Selector
* How to Assign Label on Worker Node?
  ```
  kubectl label nodes [node_name] [label-key]=[label-value]
  ```
* How to assign `nodeSelector` on Pod Spec?
  ``` yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
    labels:
      env: test
  spec:
    containers:
    - name: nginx
      image: nginx
      imagePullPolicy: IfNotPresent
    # 設定的地方在 pod.nodeSelector
    nodeSelector:
      disk_type: ssd
  ```

* How to list all node's label?
```
kubectl get nodes --show-labels
```
