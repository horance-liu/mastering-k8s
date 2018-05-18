# Kubernetes Architecture

![](media/15263629450158.png)


Kubernetes system components communicate only with the API server. They don’t talk to each other directly. The API server is the only component that communicates with etcd. None of the other components communicate with etcd directly, but instead modify the cluster state by talking to the API server.

While multiple instances of etcd and API server can be active at the same time and do perform their jobs in parallel, only a single instance of the Sched- uler and the Controller Manager may be active at a given time—with the others in standby mode.

The Control Plane components, as well as kube-proxy, can either be deployed on the system directly or they can run as pods.

The Kubelet is the only component that always runs as a regular system compo- nent, and it’s the Kubelet that then runs all the other components as pods. To run the Control Plane components as pods, the Kubelet is also deployed on the master. 


![](media/15263716980662.jpg)

## First App

### 创建一个Pod

```
$ kubectl run kubia --image=luksa/kubia --port=8080 --generator=run/v1
```

```
$ kubectl get pods
```

```
$ kubectl get pods -o wide
```

```
$ kubectl describe pod kubia-hczji
...
```

![](media/15264361701130.png)

## 创建LB

```
$ kubectl expose rc kubia --type=LoadBalancer --name kubia-http
service "kubia-http" exposed
```

```
$ kubectl get services
```


```
$ curl 104.155.74.57:8080
You’ve hit kubia-4jfyf
```

![](media/15264376618220.png)


### 扩容

```
$ kubectl scale replicationcontroller kubia --replicas=3
```


```

$ kubectl get pods

![](media/15264373562549.png)

负载均衡

```
$ curl 104.155.74.57:8080
```
