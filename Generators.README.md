# What are the names of the two secret generators defined in the kustomization.yaml file?

secretGenerator:
  - name: db-creds
    literals:
      - username=postgres
      - password=password1

  - name: domain-name
    literals:
      - domain=api.kodekloud.com
Ans :- db-creds , domain-name

# What will the value of the POSTGRES_PASSWORD environment variable be in the postgres-deployment?

secretGenerator:
  - name: db-creds
    literals:
      - username=postgres
      - password=password1

  - name: domain-name
    literals:
      - domain=api.kodekloud.com
   
 Ans :- password1


# A configmap generator named keycloak-config has two different literals:

http-port=80
https-port=443


Create 2 environment variables on the keycloak deployment container and name them the following:

KEYCLOAK_HTTP_PORT
KEYCLOAK_HTTPS_PORT


Set KEYCLOAK_HTTP_PORT to the http-port literal from the generator and set KEYCLOAK_HTTPS_PORT to the https-port literal from the generator

Ans :- In the keycloak-depl.yaml file add the below details 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: keycloak-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: keycloak
  template:
    metadata:
      labels:
        component: keycloak
    spec:
      containers:
        - name: keycloak
          image: bitnami/keycloak
          env:
            - name: KEYCLOAK_HTTP_PORT
              valueFrom:
                configMapKeyRef:
                  name: keycloak-config
                  key: http-port
            - name: KEYCLOAK_HTTPS_PORT
              valueFrom:
                configMapKeyRef:
                  name: keycloak-config
                  key: https-port    

In the container section we have added two environment variables as HTTP_PORT & HTTPS_PORT respectuvely with all required details .
Then build the kustomize file 

kustomize build k8s

Apply the configuration as follows :-

kubectl apply -k /root/code/k8s/

# Create a configMapGenerator named nginx-config that imports the nginx.conf file.

In the kustomization.yaml file
resources:
  - api-depl.yaml

configMapGenerator:
   - name: nginx-config
     files:
        - nginx.conf

It importing all of the defined config from nginx.conf from the file name nginx-config 

# Mount the nginx.conf file in the api-deployment nginx container in the following path:
/etc/nginx/conf.d/

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
        - name: api
          image: nginx
          volumeMounts:
             - name: config-volume
               mountPath: /etc/nginx/conf.d/
      volumes:
        - name: config-volume
          configMap:
            name: nginx-config         

# Please complete the following steps to move on to the next part:

# Step 1:

# In the kustomization.yaml file, there is one secretGenerator db-creds. Add a label app-config: my-config to the generator.

Step 2:

# Apply the changes to the cluster.

secretGenerator:
  - name: db-creds
    literals:
      - username=postgres
      - password=password1
    options:  
      labels:
        app-config: my-config

Then execute to apply the kustomize config 
kubectl apply -k /root/code/k8s/
secret/db-creds-t978b7h87b configured
        
# Let's change the value of the password literal to IamTheChange. After the above step, please ensure to apply the configuration to the cluster.

secretGenerator:
  - name: db-creds
    literals:
      - username=postgres
      - password=IamTheChange   # changed value
    options:
      labels:
        app-config: my-config

 How many secrets are present in default namespace?

 k get secrets -n default

 k get secrets -n default
NAME                     TYPE     DATA   AGE
domain-name-ch868d744m   Opaque   1      19m
db-creds-t978b7h87b      Opaque   2      19m
db-creds-5fh2424gk5      Opaque   2      96s

Step 5:
This time let's change the value of username literal in the db-creds secret to brandNew.

Step 6:
Now execute the following command:

kubectl apply -k /root/code/k8s --prune -l app-config=my-config

secretGenerator:
  - name: db-creds
    literals:
      - username=brandNew     # changed value
      - password=IamTheChange   
    options:
      labels:
        app-config: my-config

Now let's apply the changes:

kubectl apply -k /root/code/k8s/

Prune unused labels :-  kubectl apply -k /root/code/k8s/ --prune -l app-config=my-config


        



                  
   

        
