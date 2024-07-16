In this lesson, we will review 4 objects, which are:
- role
- roleBinding
- clusterRole
- clusterRoleBinding


RBAC is a powerful technique used to enforce security in your cluster. granting and withdrawing of priviledges to who and what action can be performed on which resource can be controlled and monitored in the cluster. 

role is a namespace scoped rbac object used to define the what action and on what resource is can be performed and mapped to a roleBinding object in which is used to define the Who can perform such tasks


[follow the link below for more](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#restrictions-on-role-creation-or-update)


clusterRole on the other hand is a cluster scoped rbac object used to define the what action and on what resource is can be performed and mapped to a clusterRoleBinding object in which is used to define the Who can perform such tasks.

In addition, when used with rolebinding, as opposed clusterrolebinding, its previledges are scoped to the namespace



[follow the link below for more](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#restrictions-on-role-creation-or-update)

example of clusterrole and rolebinding below

```

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata: 
  name: role-veiwer

rules:
- apiGroup: ["rbac.authorization.k8s.io"]
  resources: ["clusterroles"]
  verbs: ["get","list","watch"]


- apiGroup: ["rbac.authorization.k8s.io"]
  resources: ["rolebindings"]
  verbs: ["get"]


---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata: 
  name: roleveiwer-bind
  namespace: analyst

roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: role-viewer

subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: user
  name: analyst-1 


```


Stepss
1- create an azure ad user 'analyst-1'

2- write and save role and binding manifest files

3- create and deploy pods in 'analyst' namespace

4- try performing allowed and restricted tasks

5- cleanup resources

