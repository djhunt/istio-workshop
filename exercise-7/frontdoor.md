keep the simple-ingress.
check the zone of your cluster
`bx cs cluster-get ["name"]`
Change the zone in your guestbook/frontdoor-ingress.yaml if necessary. The current zone is "us-south".

get the secret of istio in current(default) name space.
`kubectl get secret -n default`
you should see something similar:
```
...
...
istio.default             istio.io/key-and-cert                 3         5d
```
add that secret to istio-system namespace:
```
kubectl get secret istio.default -o yaml | sed 's/default/istio-system/g' | kubectl -n istio-system create -f -
```
Now create the frontdoor ingress:
```sh
cat guestbook/frontdoor-ingress.yaml| sed 's/xxxx/[clustername]/g' | sed 's/ssss/istio.istio-system/g' \
| kubectl -n istio-system create -f -
```
See the result
`kubectl describe ingress -n istio-system`


