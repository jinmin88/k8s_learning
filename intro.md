# 基本概念
1. 一台機器稱做一個`node`(or minion)
1. 一個`cluster` 是多個`node`的集合
1. K8S Components
	1. `API server`: The frontend of k8s, manage users / devices / command line interface.
	1. `etcd`: A distributed reliable key value store, to store all data used to manage the cluster.
	1. `scheduler`: Resposible for distributing work or containers across multiple nodes.
	1. `controller`: They are resposible for noticing and responding when nodes, containers or end point goes down.
	1. `controoler runtime`: Is the underlying software that is used to run containers, For ex: Docker.
	1. `kubelet`: Runs on each node in the cluster. The agent is resposible for making sure that the containers are running on the nodes as expected. And provide information of worker nodes.
1. Two type of server:
	1. `Master Node`: It has `API server` / `etcd` / `controller` / `scheduler`
	2. `Worker Node`: It has `kubelet` and `controller runtime`
