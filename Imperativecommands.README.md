# Using imperative commands, add a namespace transformer to the kustomization.yaml to set the namespace to production.

imperative command :-
kustomize edit set namespace production

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - api-depl.yaml
  - api-service.yaml
  - db-depl.yaml
  - db-service.yaml
namespace: production

# Using imperative commands, add a commonLabels transformers to add the following two labels to all Kubernetes configs:

role:admin
org:kodekloud

Navigate to the respective directory and execute the below command 
controlplane ~/code/k8s ➜  kustomize edit set label role:admin org:kodekloud

New changes that were added to kustomization.yaml

commonLabels:
  org: kodekloud
  role: admin

# Use imperative commands to configure a transformer to change the nginx image to a memcached image.

 kustomize edit set image nginx=memcached

 In kustomization.yaml the new Tagname automatically appeared 

 images:
- name: nginx
  newName: memcached

  $ kustomize edit set replicas api-deployment=8


# Use imperative commands to change the replicaCount on the api-deployment to 8.

 kustomize edit set replicas api-deployment=8

 replicas:
  - count: 8
    name: api-deployment

# Use imperative commands to create a configmap generator with the following parameters:

Name: domain-config
Literals
domain=kodekloud.com
subdomain=api

Ans :- kustomize edit add configmap domain-config --from-literal=domain=kodekloud.com --from-literal=subdomain=api

# Use imperative commands to create a secret generator with the following parameters:
Name: my-secret
Literals
username=root
password=rootpassword123

A ns :-  kustomize edit add secret my-secret --from-literal=username=root --from-literal=password=rootpassword123
It will output as follows :-
secretGenerator:
  - literals:
      - username=root
      - password=rootpassword123
    name: my-secret
    type: Opaque

# Use imperative commands to add the file prometheus-depl.yaml to the list of resources in the kustomization.yaml file.

kustomize edit add resource prometheus-depl.yaml

The prometheus-deply.yaml file will appear in the base kustomization.yaml file 

resources:
  - api-depl.yaml
  - api-service.yaml
  - db-depl.yaml
  - db-service.yaml
  - prometheus-depl.yaml





