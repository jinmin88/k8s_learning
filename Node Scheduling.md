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
* `nodeSelector`'s disadvantage?
  * 只能用key=value的方式選擇我要在哪個pod上run，無法設定條件，若要設定條件，則使用`nodeAffinity`。

## Node affinity
* Node affinity has two categories:
  * `requiredDuringSchedulingIgnoredDuringExecution`
  * `perferredDuringSchedulingIgnoredDuringExecution`
  * `requiredDuringSchedulingRequiredDuringExecution`
* It's consist of three parts:
  * `requiredDuringScheduling`: 一定要node符合條件，才會把pod分派上去
  * `perferredDuringScheduling`: 會盡量嘗試尋找符合條件的node，但是不強制
  * `IgnoredDuringExecution`: 表示不會影像正在運作中的pod
* node affinity example:
  ``` yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: with-node-affinity
  spec:
    affinity:
      nodeAffinity:
        # 一定要滿足以下條件才可作 pod scheduling
        requiredDuringSchedulingIgnoredDuringExecution:
          # nodeSelector 的條件定義
          nodeSelectorTerms:
          - matchExpressions:
            # node 一定有帶有 以下任何一種 label 才可以
            # "kubernetes.io/e2e-az-name=e2e-az1"
            # "kubernetes.io/e2e-az-name=e2e-az2"
            - key: kubernetes.io/e2e-az-name
              operator: In
              values:
              - e2e-az1
              - e2e-az2
        # 儘量滿足以下條件即可作 pod scheduling
        preferredDuringSchedulingIgnoredDuringExecution:
        # 這是屬於 prefer 的權重設定(1-100)，符合條件就會得到此權重值
        # pod 會被分配到最後加總數值最高的 node
        - weight: 1
          preference:
            matchExpressions:
            # 儘量尋找帶有 label "another-node-label-key=another-node-label-value" 的 node
            - key: another-node-label-key
              operator: In
              values:
              - another-node-label-value
    containers:
    - name: with-node-affinity
      image: k8s.gcr.io/pause:2.0
  ```
