# Node Scheduling
## Taints and Tolerations
* How to assign **taint** to a node?
  ```
  kubectl taint nodes [node_name] [key]=[value]:[effect]
  ```
* How to remove **taint**?
	```
	kubectl taint nodes [node_name] [key]:[effect]-
	```
* What is `[effect]`?
