本次练习请对照《k8s in action》第三章. 请将本次练习答案上传至github远端仓库.答案包括所有yaml文件及command
Part1 pod
请简述：
1. 什么是node？
A node is a worker machine in Kubernetes. A node may be a VM or physical machine, depending on the cluster. Each node contains the services necessary to run pods and is managed by the master components. The services on a node include the container runtime, kubelet and kube-proxy.

2. 什么是pod?
Pod is a basic building block of Kubernetes, the smallest unit of Kubernetes.
Pod is a collection of containers that can run on a host. This resource is created by clients and scheduled onto hosts.

3. pod和node是什么关系？
When a Pod gets created, it is scheduled to run on a Node in your cluster. And all containers in a pod run on a same node.

4. pod和docker container是什么关系？
A pod contains one or multiple containers.

基于第三次作业请回答以下问题：
* 当前集群有几个node?
kubectl get node

* 当前集群有几个pod？
kubectl get pods --all-namespaces

* 怎么查询asp.net core app所在的pod运行在哪个node上？
kubectl get pod [$pod-name] -o wide

Part2 labels
请简述：
* 什么是labels？
Labels are key/value pairs that are attached to objects, such as pods.
Each object can have a set of key/value labels defined. Each Key must be unique for a given object.

* labels有什么用
Labels can be used to organize and to select subsets of objects. Labels can be attached to objects at creation time and subsequently added and modified at any time.

请分别通过yaml文件和command，给任意pod加上以下标签：hello-label=world
* yaml
```
"meteadata": {
    "labels": {
        "hello-label": world
    }
}
```
* command
 ```
 kubectl label pods $pod-name hello-label=world
 kubectl get pods [$pod-name] -L hello-label  //get pods with hello-label info
 ```

请分别通过yaml文件和command，将上述标签修改为：hello-label=universe
* yaml

modify yarml and ```kubectl apply -f $file-name```

* command

```
kubectl label --overwrite pod $pod-name hello-label=universe
```

请使用command，查询：
* 含有标签"hello-label"的所有pod
```
kubectl get pods -l 'hello-label'
```
* 不含有标签"hello-label"的所有pod
```
kubectl get pods -l 'hello-label'
```
* 含有标签"hello-lable"且值为"universe"的所有pod
```
kubectl get pods -l hello-label=universe
```
* 含有标签"hello-lable"且值不为"universe"的所有pod
```
kubectl get pods -l 'hello-label',hello-label!=universe
```
请使用command删除含有标签"hi-label"的所有pod
kubectl delete pods -l 'hello-label'

part3 namespace
请简述：
* 什么是namespace？
Kubernetes namespaces provide a scope for objects names

Node resource, which is global and not tied to a single namespace.

* namespace有什么用？
> Using multiple namespaces allows you to split complex systems with numerous components into smaller distinct groups. Split objects into non-overlapping groups. Such as splitting up resources into production, development, and QA environments.

> Resource names only need to be unique within a namespace. Two different namespaces can contain resources of the same name.

增
* 请分别通过yaml文件和command，创建一个名为world的namespace
```
kubectl create -f namespace-yan
```

```
kubectl create namespace namespace-yan
```
* 请分别通过yaml文件和command，创建一个pod使其隶属于上述创建的namespace

```
kubectl create -f pod-namespace-yan.yaml  // -n == -namespace
```

```
kubectl run  nginx-command --image=nginx -n namespace-yan
```

改
* 请分别通过yaml文件和command，将上述namespace的名字修改为universe
Can not rename

查
* 请使用command，查询所有namespace
```
kubectl get ns
```

* 请使用command，查询universe中的所有pod
```
kubectl get pods -n $namespace-name
```

删
* 请使用command，删除universe中的所有pod，但保留该namespace
```
kubectl -n universe delete pods --all
```

* 请使用command，删除universe namespace以及其所有pod
```
kubectl delete ns universe
```
