# Exercise 4 - Scaling in and out

### Scale the number of Hello World service pods

1. Scale the number of replicas of your Hello World service by running the following commands:

    ```sh
    kubectl get deployment

    NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    helloworld-service-v1   1         1         1            1           1m
    ```

    ```sh
    kubectl scale deployment helloworld-service-v1 --replicas=4
    ```

    ```sh
    kubectl get deployment

    NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    helloworld-service-v1   4         4         4            4           1m
    ```

    ```sh
    kubectl get pods

    NAME                          READY     STATUS    RESTARTS   AGE
    helloworld-service-v1-...    1/1       Running   0          1m
    helloworld-service-v1-...    1/1       Running   0          1m
    helloworld-service-v1-...    1/1       Running   0          1m
    helloworld-service-v1-...    1/1       Running   0          2m
    ```

2. Try scaling out further.

    ```
    kubectl scale deployment helloworld-service-v1 --replicas=12
    ```

If you look at the pod status, some of the pods will show a `Pending` state. That is because we only have four physical nodes, and the underlying infrastructure has run out of capacity to run the containers with the requested resources.

3. Pick a pod name that has a `Pending` state to confirm the lack of resources in the detailed status.

    ```
    kubectl describe pod helloworld-service...
    ```

4. Verify that the new instance has joined the Kubernetes cluster.

    ```
    kubectl get nodes
    ```
    ```
    kubectl scale deployment helloworld-service-v1 --replicas=4
    ```

5. Clean up your resources.

    ```
    kubectl delete deployment helloworld-service-v1
    ```
    
    ```
    kubectl delete svc helloworld-service
    ```  

#### [Continue to Exercise 5 - Installing Istio](../exercise-5/README.md)
