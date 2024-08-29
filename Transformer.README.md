# What is the label that will get assigned to every Kubernetes resource within /root/code/k8s/ project?

Root Kustomization.yaml:-
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - db/
  - monitoring/
  - nginx/

commonLabels:
  sandbox: dev

Ans :- sandbox:dev

What is the name that will be prefixed before all database resources?
data -

Assign the following annotation to all nginx and monitoring resources:
Just assign the annotations as below in the nginx & monitoring resources 

Nginx 
commonAnnotations:
   owner: bob@gmail.com
Monitoring 
commonAnnotations:
   owner: bob@gmail.com

# Transform all postgres images in the project to mysql.

images:
  - name: postgres
    newName: mysql


# Transform all nginx images in the nginx directory to nginx:1.23.

 images:
  - name: nginx
    newTag: "1.23"


    

   
