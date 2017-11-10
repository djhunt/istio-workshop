## Exercise 9 - Fault Injection and Rate Limiting

#### Overview of Istio Mixer

https://istio.io/docs/concepts/policy-and-control/mixer.html


#### Service Isolation Using Mixer

Block Access to the hello world service by adding the mixer-rule-denial.yaml rule shown below:

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

The reuslt will be different:
```
 {"greeting":{"greeting":"Unable to connect"},"allMessages":[{"message":"Guestbook Service is currenctly   
 unavailable","username":"system"}]}
```
Before we move to the next step, be sure to clear up this policy:
```
istioctl delete -f guestbook/mixer-rule-denial.yaml
```

#### Block Access to v2 of the hello world service

```
    rules:
      - selector: destination.labels["app"]=="helloworld-ui" && source.labels["version"] == "v2"
        aspects:
        - kind: denials
```

```
    istioctl create -f guestbook/mixer-rule-denial-v2.yaml
```

Then we clean up the rules to get everything working again:

```
  istioctl delete -f guestbook/mixer-rule-denial.yaml
  istioctl delete -f guestbook/mixer-rule-denial-v2.yaml
```

#### [Continue to Exercise 10 - Telemetry and Rate Limiting with Mixer](../exercise-10/README.md)
