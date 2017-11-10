## Exercise 4 - Scaling In and Out

#### Scale the number of Hello World Service “pods

1 - Scaling the number of replicas of our Hello World service is as simple as running :

`kubectl get deployment`

`kubectl scale deployment helloworld-service-v1 --replicas=4`

`kubectl get deployment`

`kubectl get pods`

2 - Scale out even more

`kubectl scale deployment helloworld-service-v1 --replicas=12`

If you look at the pod status some of the Pods will show a `Pending` state.   That is because we only have four physical nodes, and the underlying infrastructure has run out of capacity to run the containers with the requested resources.

3 - Pick a Pod name that is associated with the Pending state to confirm the lack of resources in the detailed status:

`kubectl describe pod helloworld-service...`

4 - Verify the new instance has joined the Kubernetes cluster, you’ll should be able to see it with this command:

`kubectl get nodes`

`kubectl scale deployment helloworld-service-v1 --replicas=4`

5 - Clean up
```
  kubectl delete deployment helloworld-service-v1
  kubectl delete svc helloworld-service
```

#### [Continue to Exercise 5 - Installing Istio](../exercise-5/README.md)
