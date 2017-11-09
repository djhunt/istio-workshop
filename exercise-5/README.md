## Exercise 5 - Installing Istio

#### Download Istio

Either download it directly or get the latest using curl:

https://github.com/istio/istio/releases

```
curl -L https://git.io/getLatestIstio | sh -

export PATH=$PWD/istio-0.2.12/bin:$PATH
```

#### Running istioctl

Istio related commands need to have `istioctl` in the path.  Verify it is available by running:

`istioctl -h`


#### Install Istio on the Kubernetes Cluster

1 - First grant cluster admin permissions to the current user (admin permissions are required to create the necessary RBAC rules for Istio):
``` 
    bx account users
```
Take the output to the next command
<pre>
    kubectl create clusterrolebinding cluster-admin-binding \
        --clusterrole=cluster-admin \
        --user=<i><b>user</b></i>
</pre>
2 - Next install Istio on the Kubernetes cluster:

Change to the Istio directory (istio-0.2.12) and and install istio in the kubernetes cluster

```
    cd istio-0.2.12
    kubectl apply -f install/kubernetes/istio.yaml
```


#### Viewing the Istio Deployments

Istio is deployed in a separate Kubernetes namespace `istio-system`  You can watch the state of Istio and other services and pods using the watch command.  For example in 2 separate terminal windows run:

```
  kubectl get po --all-namespaces
  kubectl get svc --all-namespaces
```
### clone this repo

Get out of the istio directory and back to the termnial. run:
```
    git clone https://github.com/szihai/istio-workshop.git
    
    cd istio-workshop
```
This is the working directory for the lab


#### [Continue to Exercise 6 - Creating a Service Mesh with Istio Proxy](../exercise-6/README.md)
