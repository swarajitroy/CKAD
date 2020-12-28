# Services & Networking
---

* Understand Services
   * Cluster IP
   * NodePort
   * Load Balancer
   
* 02. Demonstrate basic understanding of NetworkPolicies
   * 02.A Deny all traffic to a pod



## 02. Demonstrate basic understanding of NetworkPolicies
---

### 02.A Deny all traffic to a pod
---

| ID | Task | Description |
| 01 | Create Application | Create a Pod with label app=web - this could be a pod, or a replicaset or deployment |
| 02 | Apply NetworkPolicy| The network policy will reject all traffic incoming to the application pods |
| 03 | Test Setup | Deploy a busybox pod and test via wget or curl | 


 



## Links
---

https://github.com/ahmetb/kubernetes-network-policy-recipes
