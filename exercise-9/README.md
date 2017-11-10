## Exercise 9 - Fault Injection and Rate Limiting

#### Overview of Istio Mixer

https://istio.io/docs/concepts/policy-and-control/mixer.html


#### Service Isolation Using Mixer

Block Access to the guestbook backend service by adding the mixer-rule-denial.yaml rule shown below:

```
  apiVersion: "config.istio.io/v1alpha2"
  kind: denier
  metadata:
    name: denyall
    namespace: istio-system
  spec:
    status:
      code: 7
      message: Not allowed
  ---
  apiVersion: "config.istio.io/v1alpha2"
  kind: checknothing
  metadata:
    name: denyrequest
    namespace: istio-system
  spec:

  ---
  apiVersion: "config.istio.io/v1alpha2"
  kind: rule
  metadata:
    name: mixerdenysome
    namespace: istio-system
  spec:
    match: source.labels["app"]=="guestbook-ui"
    actions:
    # handler and instance names default to the rule's namespace.
    - handler: denyall.denier
      instances: [ denyrequest.checknothing.istio-system ]
```
To apply this policy, run:

```
    istioctl create -f guestbook/mixer-rule-denial.yaml
```
Now run the curl again:
```
curl http://169.47.103.138/echo/universe
```

The result will be different:
```
 {"greeting":{"greeting":"Unable to connect"},"allMessages":[{"message":"Guestbook Service is currenctly   
 unavailable","username":"system"}]}
```
Before we move to the next step, be sure to clean up this policy:
```
istioctl delete -f guestbook/mixer-rule-denial.yaml
```

#### Block Access to v2 of the hello world service

```
  apiVersion: "config.istio.io/v1alpha2"
  kind: denier
  metadata:
    name: denyall
    namespace: istio-system
  spec:
    status:
      code: 7
      message: Not allowed
  ---
  apiVersion: "config.istio.io/v1alpha2"
  kind: checknothing
  metadata:
    name: denyrequest
    namespace: istio-system
  spec:

  ---
  apiVersion: "config.istio.io/v1alpha2"
  kind: rule
  metadata:
    name: mixerdenysome
    namespace: istio-system
  spec:
    match: destination.labels["app"]=="helloworld-service" && destination.labels["version"] == "2.0"
    actions:
    # handler and instance names default to the rule's namespace.
    - handler: denyall.denier
      instances: [ denyrequest.checknothing.istio-system ]

```
To apply the policy, run:
```
    istioctl create -f guestbook/mixer-rule-denial-v2.yaml
```
Let's verify the result:
```
curl http://169.47.103.138/hello/world
{"hostname":"helloworld-service-v1-5c95ddd467-txgld","greeting":"Hello world from helloworld-service-v1-5c95ddd467-txgld 
with 1.0","version":"1.0"}

curl http://169.47.103.138/hello/world
PERMISSION_DENIED:denyall.denier.istio-system:Not allowed
```
Note: only version "2.0" is blocked while verison "1.0" works fine. Also the denial message is different from before. This is because in this case we directly blocked the helloworld service and the client gets the denial message from mixer. In the previous example, we blocked the backend service to guestbook-ui service, which returned an elegant response.

Again, we clean up the policy to get everything working again:

```
  istioctl delete -f guestbook/mixer-rule-denial-v2.yaml
```

#### [Continue to Exercise 10 - Telemetry and Rate Limiting with Mixer](../exercise-10/README.md)
