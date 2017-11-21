# Access Kubernetes cluster
Your IBM Cloud paid account and your Kubernetes cluster have been pre-provisioned for you. Here are the steps to access your cluster:

## Install IBM Container Service command line utilities

* Download and install the latest IBM Cloud CLI from [here](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started).   
* Login to Bluemix using command:         
  `bx login`   
* Install the Container Service plugin.   
  `bx plugin install container-service -r Bluemix`   
* Verify that you have the container-service plugin installed.   
  `bluemix plugin list`   
* Initialize the Container Service Plugin.   
  `bx cs init`  
* Install the latest Kubernetes command line [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).   

## Access your cluster

- Set the Kubernetes environment to work with your cluster. Run:
```
  bx cs cluster-config [your_cluster_name]
  export KUBECONFIG=/Users/ibm/.bluemix/plugins/cs-cli/clusters/wordpress/[config_name].yml
```
- Create a proxy to your K8s API server:   
```
  kubectl proxy
```
Now in a browser, go to [http://localhost:8001/ui](http://localhost:8001/ui) to access the API server dashboard.

#### [Continue to Exercise 2 - Deploying a microservice to Kubernetes](../exercise-2/README.md)
