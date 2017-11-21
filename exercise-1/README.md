# Exercise 1 - Accessing a Kubernetes cluster with IBM Cloud Container Service

## Access your cluster

1. Set the context for your cluster in your CLI. Every time you log in to the IBM Bluemix Container Service CLI to work with the cluster, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in IBM Cloud.

    a. Download the configuration file and certificates for your cluster using the `cluster-config` command.
    ```bash
    bx cs cluster-config [your_cluster_name]
    
    export KUBECONFIG=/Users/ibm/.bluemix/plugins/cs-cli/clusters/wordpress/kube-config-dal10-wordpress.yml
    ```

    b. Copy and paste the command from the previous step to set the `KUBECONFIG` environment variable and configure your CLI to run `kubectl` commands against your cluster.

2. Create a proxy to your Kubernetes API server.

    ```
    kubectl proxy
    ```
    
3. In a browser, go to http://localhost:8001/ui to access the API server dashboard.

#### [Continue to Exercise 2 - Deploying a microservice to Kubernetes](../exercise-2/README.md)
