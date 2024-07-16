# PART I

persistency in deployment still remains top priority for every devops team, which is why we will explore different types of storages and database management inclusive.
This project demonstrates a stateful app deployment, consisting the following files.

 - app.yaml : this file contains the deployment and service configuration of the app

 - db.yaml: this file contains the database deployment and service configuration, using the mysql official published image

 - configmap.yaml: this file contains data mounted as volume and secret in the database configuration file 

 - storage.yaml: this file contains the storage configuration for the project
 using a custom storage class, persistent volume claim and persistent volume using azure disks as the storage provisioner.

 ### what are storage classes, persistent volumes and persistent volume claim

Storage Class: defines how and what storage resources are provisioned and managed

Persistent Volume: this is the resource that represents a piece of networked storage, provisioned and managed by kubernetes

Persistent Volume claim: is a request for storage resources, its a way for pod to request for storage from a persistent volume


Note:

In this project we explored both managed and custom defined storage classes

paying a close attention to storage.yaml, you'll notice two seperate pvc was defined with just one storage class, yes its correct. we trying to learn 2 different ways to use storage classes

i- custom-sc: this was a customly created storage class used in the custom pvc for the deployments

ii- managed-sc: aks by default provides 4 storage classes by default, to check this on your cluster, run the command below

`  kubectl get sc `

which can be used in your persistent volume claim configuration for your deployments


References:

[kubernetes documentation](https://kubernetes.io/docs/reference/)
[azure disks documetation for aks](https://learn.microsoft.com/en-us/azure/aks/azure-csi-disk-storage-provision)
[kubernetes api reference documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#-strong-api-overview-strong-)


---

above are some of the links, used to complete the project.

Remember to always check the documentation when writing your manifest files to avoid deprecated settings and errors 

cheers!!!


# PART II

In this project we will we use azure mysql in place of azure disks, due to several challenges 
like:

 - complex setup to achieve HA
 - complex master-master, master-slave mysql setup
 - no auto backup and recovery
 - no auto-upgrade mysql
 - logging, monitoring needs custom scripts


why use Azure MySQL database ?
 - built in HA with no extra cost
 - scale as needed
 - auto backup and recovery
 - highly secure
 - monitoring and alerting available


 ### STEPS

 *Step 1: Create an azure mysql database*

 - search azure database for mysql servers and create the resource
 - once created, go to settings >> connection security, and enable allow azure services
 - copy the database endpoint

 *Step 2: create Kubernetes external services and other manifest files*

 *Step 3: *


 NoTE: we donot have configuration files for mysql deployments and service because its provisioned by aks for us, with so many advantages leveraged in the project



 REFERENCE
 [kubernetes documentation](https://kubernetes.io/docs/reference/)
 
 [azure file storage docs for aks](https://learn.microsoft.com/en-us/azure/aks/azure-csi-files-storage-provision)

 [kubernetes api reference documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#-strong-api-overview-strong-)


---


similar to the 01-azure-disks project, we are substituting azure disks for azure 
files in the project



REFERENCES
[kubernetes documentation](https://kubernetes.io/docs/reference/)

[azure file storage docs for aks](https://learn.microsoft.com/en-us/azure/aks/azure-csi-files-storage-provision)

 [kubernetes api reference documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#-strong-api-overview-strong-)