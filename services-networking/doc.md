# Services & Networking
---

* Understand Services
   * Cluster IP
   * NodePort
   * Load Balancer
   
* 02. Demonstrate basic understanding of NetworkPolicies
   * [02.A Deny all traffic to a pod](#02a-deny-all-traffic-to-a-pod)



## 02. Demonstrate basic understanding of NetworkPolicies
---

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

#### 02.A.0B Apply NetworkPolicy
---

## Links
---

https://github.com/ahmetb/kubernetes-network-policy-recipes
