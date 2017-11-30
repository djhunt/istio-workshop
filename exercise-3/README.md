# Exercise 3 - Creating a Kubernetes service

Each pod has a unique IP address, but the address is ephemeral. The pod IP addresses are not stable and can change when pods start or restart. A service provides a single access point to a set of pods matching some constraints. A service IP address is stable.

In Kubernetes, you can instruct the underlying infrastructure to create an external load balancer by specifying the service type as a LoadBalancer. If you open up [helloworldservice-service.yaml](helloworldservice-service.yaml), you will see that it has `type: LoadBalancer`.

### Create a LoadBalancer service

1. Create a LoadBalancer service for the Hello World service.

    ```
    kubectl apply -f kubernetes/helloworldservice-service.yaml --record
    ```

2. Verify that Load Balancer service was created. The `EXTERNAL IP` of the LoadBalancer will start as pending and will be populated after a short period.

    ```bash
    kubectl get services
    
    NAME                 CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
    helloworld-service   172.21.48.166   169.47.103.138   8080:32678/TCP   1m
    ```

### Test the Hello World service

Curl the external IP address to test the Hello World service.

    ```
    curl [EXTERNAL-IP]:8080/hello/world
    ```

#### Optional - Curl the service using a DNS name

If you log in to another container, you can access the Hello World service via the DNS name. For example, start `tutum/curl` to get a shell and curl the service using the service name:

```
kubectl run curl --image=tutum/curl -i --tty

root@busybox:/data# curl http://helloworld-service:8080/hello/Batman
{"greeting":"Hello Batman from helloworld-service-... with 1.0","hostname":"helloworld-service-...","version":"1.0"}

root@busybox:/data# exit
```

## Explanation
#### By Ray Tsang [@saturnism](https://twitter.com/saturnism)

Open the [helloworldservice-service.yaml](helloworldservice-service.yaml) to examine the service descriptor. The important part about this file is the selector section. This is how a service knows which pod to route the traffic to, by matching the selector labels with the labels of the pods.

The other important part to notice in this file is the type of service is a Load Balancer.  This tells Bluemix that an externally facing load balancer should be created for this service so that it is accessible from the outside.

Since we are running two instances of the Hello World Service (one instance in one pod), and that the IP addresses are not only unique, but also ephemeral - how will a client reach our services? We need a way to discover the service.

In Kubernetes, Service Discovery is a first class citizen. We created a Service that will:
- act as a load balancer to load balance the requests to the pods, and
- provide a stable IP address, allow discovery from the API, and also create a DNS name!

#### [Continue to Exercise 4 - Scaling in and out](../exercise-4/README.md)
