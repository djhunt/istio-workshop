## Exercise 10 - Telemetry and Rate Limiting with Mixer

#### Instal Istio Monitoring and Metrics Tools

Switch to the Istio installation directory to install the addons:

```
    kubectl apply -f ../istio-0.2.12/install/kubernetes/addons/prometheus.yaml
    kubectl apply -f ../istio-0.2.12/install/kubernetes/addons/grafana.yaml
    kubectl apply -f ../istio-0.2.12/install/kubernetes/addons/servicegraph.yaml
    kubectl apply -f ../istio-0.2.12/install/kubernetes/addons/zipkin.yaml
```
Verify that the service is running in your cluster.
`kubectl -n istio-system get svc`   

#### Adding Telemetry Rules

Make sure you are in the lab directory. Then run:

```  
  istioctl create -f telemetry_rule.yaml
```
### Verify that the new metric values are being generated and collected.

In a Kubernetes environment, setup port-forwarding for Prometheus by executing the following command:
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &   
```
Now you can generate traffic by doing the curl several times:
```
    curl http://169.47.103.138/echo/universe
```

Browse to the Grafana dashboard:

http://localhost:9090/graph
You should see the ![dashboard](). 
On the top most query box, enter the query `request_count`, you should see something like ![this]().

From the terminal, run:
```
kubectl -n istio-system logs $(kubectl -n istio-system get pods -l istio=mixer -o jsonpath='{.items[0].metadata.name}') mixer | grep \"instance\":\"newlog.logentry.istio-system\"

```
This will search through the logs for the Mixer pod. The expected output is something like
![this]()


#### Rate Limiting with Mixer

Then apply a 1 request per second rate limit from the UI to the helloworld-service

```
    istioctl create -f rate-limit-ui-service.yaml
```

Then we can drive traffic to the UI to see the rate limit in action:

```
    watch -n 0.1 curl -i <IP>:/echo/foo
```

and in grafana we can see the 429’s.
http://localhost:9090/graph
