# What components are enabled in the community overlay?
bases:
  - ../../base

components:
  - ../../components/auth

Ans :- auth

# What components are enabled in the dev overlay?
bases:
  - ../../base

components:
  - ../../components/auth
  - ../../components/db
  - ../../components/logging
Ans :- Options are above

# How many environment variables does the db component add to the api-deployment?

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: api
          env:
            - name: DB_CONNECTION
              value: postgres-service
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-creds
                  key: password
Ans : 2 

# What is the name of the secret generator created in the db component?

db -creds
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: api
          env:
            - name: DB_CONNECTION
              value: postgres-service
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-creds
                  key: password

# The community edition of the application should now ship with the logging component. Please add the logging component to the community overlay.

bases:
  - ../../base

components:
  - ../../components/auth
  - ../../components/logging

# A new caching component needs to be created. There is a directory in the components directory called caching that has a deployment and service config. Finish creating the component, by creating the kustomization.yaml file and importing the redis configurations.

apiVersion: kustomize.config.k8s.io/v1alpha1
kind: Component

resources:
  - redis-depl.yaml
  - redis-service.yaml
# With the database setup for the caching component, the component needs to update the api-deployment configuration to add the environment variable to the container to reach the redis instance. Create a Strategic merge Patch and add the following environment variable: Let's create the api-patch.yaml file and add the necessary environment variables as follows:

components/caching/api-patch.yaml



apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  template:
    spec:
      containers:
        - name: api
          env:
            - name: REDIS_CONNECTION
              value: redis-service



1. Now update the kustomization.yaml in the caching directory to add the api-patch as shown below:
components/caching/kustomization.yaml

2. patches:
  - api-patch.yaml



# Finally let's add the caching component to the enterprise edition of the application. Update the kustomization.yaml file in the enterprise overlay with the caching component as shown below:

overlays/enterprise/kustomization.yaml

components:
  - ../../components/auth
  - ../../components/db
  - ../../components/caching
























