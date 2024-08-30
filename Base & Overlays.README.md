# When deploying application to prod environment, what type of image will be used for the api-deployment?

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: api
          image: memcached

Ans :- memcached

# How many replicas for api-deployment will get deployed in prod?

2
Becuase the yaml consists of 2 replicas 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      component: api
  template:
    metadata:
      labels:
        component: api
    spec:
      containers:
        - name: api
          image: nginx
          env:
            - name: DB_CONNECTION
              value: db.kodekloud.com


# What will be the value of the environment variable MONGO_INITDB_ROOT_PASSWORD in the mongo-deployment container in the staging environment?

Ans :-
apiVersion: v1
kind: ConfigMap
metadata:
  name: db-creds
data:
  username: mongo
  password: superp@ssword123


The prod environment will deploy 2 api pods, 2 redis pods and 1 mongo pod.
overlays/prod/kustomization.yaml

resources:
  - redis-depl.yaml
overlays/prod/redis-depl.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 2

# How many environment variables are set on the nginx container in the api-deployment in dev environment?

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      component: api
  template:
    metadata:
      labels:
        component: api
    spec:
      containers:
        - name: api
          image: nginx
          env:
            - name: DB_USERNAME
              valueFrom:
                configMapKeyRef:
                  name: db-creds
                  key: username
            - name: DB_PASSWORD
              valueFrom:
                configMapKeyRef:
                  name: db-creds
                  key: password

Ans :- 2

# Update the api image in the api-deployment to use caddy docker image in the QA environment.Perform this using an inline JSON6902 patch.
Note: Please ensure to apply the updated config for QA environment before validation.

Ans :- In the kustomization.yaml file which is located at the /root/code/k8s/overlays/QA directory, add a json6902 patch to update the image as shown below:



overlays/QA/kustomization.yaml



patches:
  - target:
      kind: Deployment
      name: api-deployment
    patch: |-
      - op: replace
        path: /spec/template/spec/containers/0/image
        value: caddy



After modifications, apply the changes to create updated deployments in QA environment:



$ kubectl apply -k /root/code/k8s/overlays/QA


# A mysql database needs to be added only in the staging environment. Create a mysql deployment in a file called mysql-depl.yaml and define the deployment name as mysql-deployment. Deploy 1 replica of the mysql container using mysql image and set the following env variables:

- name: MYSQL_ROOT_PASSWORD
  value: mypassword

NOTE: Please ensure to deploy the changes committed in the staging environment before validation.

Ans :- Create a mysql-deply.yaml under staging directory and provide the below manifests 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: mysql
  template:
    metadata:
      labels:
        component: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: mypassword

  Now update the staging environment kustomization.yaml file to include the deployment file we created earlier:

 Under  overlays/staging/kustomization.yaml 
 resources:
  - mysql-depl.yaml

Applying the changes to k8s cluster for staging environment:
kubectl apply -k /root/code/k8s/overlays/staging


























          









