## Exercise 1 - Startup a Kubernetes Cluster

1. Create your free Kubernetes cluster
    ```bash
    $ bx cs cluster-create --name [your_cluster_name]
    ```
    A free cluster comes with one worker node to deploy container pods upon. A worker node is the compute host, typically a virtual machine, that your pods run on. A pod is a group of one or more containers, the shared storage for those containers, and the options about how to run them. The pod model is as an "application specific logical host", which means it contains one or more application containers which are relatively tightly coupled.

> **Note:** It can take up to 2 hours for the worker node machine to be ordered, and for the cluster to be set up and provisioned.

2. Before you continue to the next step, verify that the deployment of your worker node is complete.
    ```bash
    $ bx cs workers [your_cluster_name]
    ID                                           Public IP       Private IP    Machine Type  State     Status   
    dal10-pa8dfcc5223804439c87489886dbbc9c07-w1  169.47.223.113  10.171.42.93  free         deployed  Deploy Automation                                          Successful   
    ```

3. Set the context for your cluster in your CLI. Every time you log in to the IBM Bluemix Container Service CLI to work with the cluster, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in Bluemix.

    a. Download the configuration file and certificates for your cluster using the `cluster-config` command.
    ```bash
    $ bx cs cluster-config [your_cluster_name]
    export KUBECONFIG=/Users/ibm/.bluemix/plugins/cs-cli/clusters/wordpress/kube-config-dal10-wordpress.yml
    ```

    b. Copy and paste the command from the previous step to set the KUBECONFIG environment variable and configure your CLI to run kubectl commands against your cluster.


## Explanation
#### By Ray Tsang [@saturnism](https://twitter.com/saturnism)

This will take a few minutes to run. Behind the scenes, it will create Google Compute Engine instances, and configure each instance as a Kubernetes node. These instances don’t include the Kubernetes Master node. In Google Container Engine, the Kubernetes Master node is managed service so that you don’t have to worry about it!

The scopes parameter is important for this lab. Scopes determine what Google Cloud Platform resources these newly created instances can access.  By default, instances are able to read from Google Cloud Storage, write metrics to Google Cloud Monitoring, etc. For our lab, we add the cloud-platform scope to give us more privileges, such as writing to Cloud Storage as well.

#### [Continue to Exercise 2 - Deploying a microservice to Kubernetes](../exercise-2/README.md)
