# How many nginx pods will get created?

resources:
  - mongo-depl.yaml
  - nginx-depl.yaml
  - mongo-service.yaml

patches:
  - target:
      kind: Deployment
      name: nginx-deployment
    patch: |-
      - op: replace
        path: /spec/replicas
        value: 3

  - target:
      kind: Deployment
      name: mongo-deployment
    path: mongo-label-patch.yaml

  - target:
      kind: Service
      name: mongo-cluster-ip-service
    patch: |-
      - op: replace
        path: /spec/ports/0/port
        value: 30000

      - op: replace
        path: /spec/ports/0/targetPort
        value: 30000
So the answer will be 3

# What are the labels that will be applied to the mongo deployment?

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: mongo
  template:
    metadata:
      labels:
        component: mongo
    spec:
      containers:
        - name: mongo
          image: mongo

Ansswer will be :- component: mongo

# What is the target port of the mongo-cluster-ip-service?

apiVersion: v1
kind: Service
metadata:
  name: mongo-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: mongo
  ports:
    - port: 27017
      targetPort: 27017

Answer will be 27017

# What is the target port of the mongo-cluster-ip-service?

resources:
  - mongo-depl.yaml
  - nginx-depl.yaml
  - mongo-service.yaml

patches:
  - target:
      kind: Deployment
      name: nginx-deployment
    patch: |-
      - op: replace
        path: /spec/replicas
        value: 3

  - target:
      kind: Deployment
      name: mongo-deployment
    path: mongo-label-patch.yaml

  - target:
      kind: Service
      name: mongo-cluster-ip-service
    patch: |-
      - op: replace
        path: /spec/ports/0/port
        value: 30000

      - op: replace
        path: /spec/ports/0/targetPort
        value: 30000
Ans :- 30000

# We just added few files along with some modifications in the k8s directory, observe the changes and answer the following questions.How many containers are in the api pod?

# We just added few files along with some modifications in the k8s directory, observe the changes and answer the following questions.How many containers are in the api pod?

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: api
  template:
    metadata:
      labels:
        component: api
    spec:
      containers:
        - name: nginx
          image: nginx
 apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: memcached
          image: memcached

So the answer will be 2

# What path in the mongo container is the mongo-volume volume mounted at?

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
spec:
  template:
    spec:
      containers:
        - name: mongo
          volumeMounts:
            - mountPath: /data/db
              name: mongo-volume
      volumes:
        - name: mongo-volume
          persistentVolumeClaim:
            claimName: host-pvc
Ans :- /data/db

# In api-patch.yaml create a strategic merge patch to remove the memcached container.

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - $patch: delete
          name: memcached
#  kubectl apply -k /root/code/k8s/
          
In this yaml file the memcached delete and nginx remains as it is .

Output :-
controlplane ~/code ✖ k apply -k /root/code/k8s/
service/mongo-cluster-ip-service unchanged
deployment.apps/api-deployment configured
deployment.apps/mongo-deployment unchanged


# Create an inline json6902 patch in the kustomization.yaml file to remove the label org: KodeKloud from the mongo-deployment.

patches:
  - target:
      kind: Deployment
      name: mongo-deployment
    patch: |-
      - op: remove
        path: /spec/template/metadata/labels/org

$ kubectl apply -k /root/code/k8s/

The actual syntax implements as follows :-

resources:
  - mongo-depl.yaml
  - api-depl.yaml
  - mongo-service.yaml

patches:
  - target:
      kind: Deployment
      name: mongo-deployment
    patch: |-
      - op: remove
        path: /spec/template/metadata/labels/org


        
        




            

            






