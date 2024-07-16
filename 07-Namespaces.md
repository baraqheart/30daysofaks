
Namespace are logical way of grouping resources in a kubernetes cluster

Resources management happens at namespace level also using:

limitsRange and ResourceQuota

LimitsRange can be used to enforce the amount of resources for each pods or container, amount of starage for each pvcs and also enforce limits and requests ratio for a resource in a namespace

example is shown below


```

apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default: # this section defines default limits
      cpu: 500m
    defaultRequest: # this section defines default requests
      cpu: 500m
    max: # max and min define the limit range
      cpu: "1"
    min:
      cpu: 100m
    type: Container



```

ResourceQuota defines the amount of resource allocated to an object in a namespace


# REFERENCES
[limit range docs](https://kubernetes.io/docs/concepts/policy/limit-range/)
[resource quota](https://kubernetes.io/docs/concepts/policy/resource-quotas/)