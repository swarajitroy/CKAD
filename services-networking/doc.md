# Services & Networking
---

* Understand Services
   * Cluster IP
   * NodePort
   * Load Balancer
   
* [02. Demonstrate basic understanding of NetworkPolicies](#02-demonstrate-basic-understanding-of-networkpolicies)
   * [02.A Deny all traffic to a pod](#02a-deny-all-traffic-to-a-pod)


## 01. Understand Services 
---


## 02. Demonstrate basic understanding of NetworkPolicies
---

Please be adviced, the desktop Kubernetes like MiniKube does not support Network Policies. Please test all the Network Policy recipes on a diffent K8S cluster.



### 02.A Deny all traffic to a pod
---

| ID | Task | Description |
| ----------- | ----------- | -------|
| 01 | Create Application | Create a Pod with label app=web - this could be a pod, or a replicaset or deployment |
| 02 | Apply NetworkPolicy| The network policy will reject all traffic incoming to the application pods |
| 03 | Test Setup | Deploy a busybox pod and test via wget or curl | 


#### 02.A.01 Create Application
---

 ```
Swarajits-MacBook-Air:GITOPS_HOME swarajitroy$ kubectl run swararoy-application --image=nginx -l app=web
deployment.apps/swararoy-application created

Swarajits-MacBook-Air:GITOPS_HOME swarajitroy$  kubectl get pod -l app=web -o wide
NAME                                    READY   STATUS    RESTARTS   AGE    IP            NODE       NOMINATED NODE   READINESS GATES
swararoy-application-547d996cc9-vkws9   1/1     Running   0          3m1s   172.17.0.16   minikube   <none>           <none>

```

This additionally creates a replicaset and deployment automatically. 

Now - lets test. Use https://kubernetes.io/docs/reference/kubectl/cheatsheet/ - the Kubernetes cheet sheet, 

```
kubectl run -i --tty busybox --image=busybox -- sh
/ # wget -qO- http://172.17.0.16:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>

Once we come out of the busybox session, we can always attach it back as long as pod is running.

Session ended, resume using 'kubectl attach busybox-6c446876c6-867wz -c busybox -i -t' command when the pod is running

```

#### 02.A.02 Apply NetworkPolicy
---

We need to design a network policy now. We have to create a Network policy on Pods which has label app=web using a PodSelector contruct, and as we have to control the incoming traffic to this pod, we need to select the ingress policy. The best way to do this to have an ingress policy blank - that way, all traffic gets stopped. As Network Policy creation is not available via Kubernetes imperative command, we need to get a good YAML from Kubernetes website.

```
Swarajits-MacBook-Air:netpol swarajitroy$ cat web-deny-all.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: web-deny-all
spec:
  podSelector:
    matchLabels:
      app: web
  ingress: []
 
Swarajits-MacBook-Air:netpol swarajitroy$ kubectl apply -f web-deny-all.yaml
networkpolicy.networking.k8s.io/web-deny-all created

Swarajits-MacBook-Air:netpol swarajitroy$ kubectl get netpol
NAME           POD-SELECTOR   AGE
web-deny-all   app=web        23s

Swarajits-MacBook-Air:netpol swarajitroy$ kubectl describe netpol web-deny-all
Name:         web-deny-all
Namespace:    default
Created on:   2020-12-28 11:45:20 +0530 IST
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"networking.k8s.io/v1","kind":"NetworkPolicy","metadata":{"annotations":{},"name":"web-deny-all","namespace":"default"},"spe...
Spec:
  PodSelector:     app=web
  Allowing ingress traffic:
    <none> (Selected pods are isolated for ingress connectivity)
  Allowing egress traffic:
    <none> (Selected pods are isolated for egress connectivity)
  Policy Types: Ingress

```

#### 02.A.03 Test Setup 
---

```

Swarajits-MacBook-Air:netpol swarajitroy$ kubectl attach busybox-6c446876c6-867wz -c busybox -i -t
If you don't see a command prompt, try pressing enter.
/ #


```

## Links
---

https://github.com/ahmetb/kubernetes-network-policy-recipes
