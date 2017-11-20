# Exercise 5 - Installing Istio

### Clean up
 
Start with a clean slate by deleting all deployed services from the cluster.

```sh
kubectl delete all --all
```

### Download Istio

1. Either download Istio directly from https://github.com/istio/istio/releases or get the latest version using curl:

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```

2. Extract the installation files.
   
3. Add the `istioctl` client to your PATH. For example, run the following command on a MacOS or Linux system:

   ```
   export PATH=$PWD/istio-0.2.12/bin:$PATH
   ```

### Install Istio on the Kubernetes cluster

1. Get the IBMid for the current user.
    
    * If you created your own cluster, run the following command and note the returned IBMid:
    ``` 
    bx account users
    ```

    * If the cluster was provisioned for you by IBM, use the IBMid provided to you.

2. Grant cluster admin permissions to the current user. Admin permissions are required to create the necessary RBAC rules for Istio.

    ```
    kubectl create clusterrolebinding cluster-admin-binding \
        --clusterrole=cluster-admin \
        --user=[IBMid]
    ```

3. Change the directory to the Istio file location.

   ```
   cd [path_to_istio-0.2.12]
   ```

3. Install Istio on the Kubernetes cluster.

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```

### Install Add-ons for Grafana, Prometheus, and Zipkin

```sh
kubectl apply -f install/kubernetes/addons/zipkin.yaml
kubectl apply -f install/kubernetes/addons/grafana.yaml
kubectl apply -f install/kubernetes/addons/prometheus.yaml
kubectl apply -f install/kubernetes/addons/servicegraph.yaml
```

### View the Istio deployments

Istio is deployed in a separate Kubernetes namespace, `istio-system`. You can watch the state of Istio and other services and pods using the watch command.  For example in 2 separate terminal windows run:

```
kubectl get po --all-namespaces
kubectl get svc --all-namespaces
```

#### [Continue to Exercise 6 - Creating a service mesh with Istio Proxy](../exercise-6/README.md)
