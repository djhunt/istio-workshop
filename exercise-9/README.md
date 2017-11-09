## Exercise 9 - Fault Injection and Rate Limiting

#### Overview of Istio Mixer

https://istio.io/docs/concepts/policy-and-control/mixer.html


#### Service Isolation Using Mixer

Block Access to the hello world service by adding the mixer-rule-denial.yaml rule shown below:

```
  rules:
    - selector: source.labels["app"]=="helloworld-ui"
      aspects:
      - kind: denials
```

```
    istioctl create -f guestbook/mixer-rule-denial.yaml
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
