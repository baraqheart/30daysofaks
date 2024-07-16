Workloads
- pods: a pod is the smallest unit of deployment in kubernetes. 
- replicasets: ensures a specified amount of pods are running at all times
- deployments manages rollouts of new application by scaling replicasets

configuration 
- configmap: is an object used to store some non sensitive information or variables which 
  could also be mounted as a volume for example

```
#declation

apiVersion:
kind: ConfigMap
metadata:
  name: my-configmap
data: 
  items: "products"
  amount: "100"
  
  items.properties: |
    products.color="red"
    products.size="large" 


#use


name: ITEMS
valueFrom:
  configMapKeyRef:
    name: my-configmap
    key: items


```


- secret: is very similar to configmap but used to store sensitive information

```
#declaration

apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:
  secret-file: dmFsdWUtMg0KDQo=


----
#use

name: SECRET
valueFrom:
  secretKeyRef:
    name: my-secret
    key: secret-file

```
- resources management

  - requests: is used to allocated the amount of resource needed by a container to run smoothly,   
    this helps the scheduler to determine which node to assign a pod
  - limits: this is the maximum amount of resources a container is allowed to use, this is 
    enforced to prevent a contained over utilizing the resources, see example below.


```
#should be added in the container detail

resources:
  requests:
    cpu: "250m"
    memory: "64Mi"
  limits:
    cpu: "500m"
    memory: "128Mi"


```


References

[secret docs](https://kubernetes.io/docs/concepts/configuration/secret/)
[configmap docs](https://kubernetes.io/docs/concepts/configuration/configmap/)