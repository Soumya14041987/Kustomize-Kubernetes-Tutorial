# Let's create a single kustomization.yaml file in the root of the k8s directory and import all resources defined for db, message-broker, nginx into it.


Please ensure to apply the config after creating kustomization.yaml file.


apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# kubernetes resources to be managed by kustomize
resources:
  - db/db-config.yaml
  - db/db-depl.yaml
  - db/db-service.yaml
  - message-broker/rabbitmq-config.yaml
  - message-broker/rabbitmq-depl.yaml
  - message-broker/rabbitmq-service.yaml
  - nginx/nginx-depl.yaml
  - nginx/nginx-service.yaml

controlplane ~/code/k8s ➜  pwd
/root/code/k8s

controlplane ~/code/k8s ➜  kubectl apply -k .
-----------OR---------------
controlplane ~/code ➜  kustomize build k8s/ | kubectl apply -f -


What is the type of the service that has been deployed for the message-broker?

ClusterIP 


# Let's create a kustomization.yaml file in each of the subdirectories and import only the resources within that directory.Please ensure to specify those directories within the root kustomization.yaml file.


NOTE: Don't forget to deploy the resources.


controlplane ~/code ➜  tree
.
└── k8s
    ├── db
    │   └── kustomization.yaml
    ├── kustomization.yaml
    ├── message-broker
    │   └── kustomization.yaml
    └── nginx
        └── kustomization.yaml
4 directories, 12 files


Under each resource sub-directories :-
resources:
  - db-depl.yaml
  - db-service.yaml
  - db-config.yaml

resources:
  - rabbitmq-config.yaml
  - rabbitmq-depl.yaml
  - rabbitmq-service.yaml

resources:
  - nginx-depl.yaml
  - nginx-service.yaml

  kustomize build /root/code/k8s/ | kubectl apply -f -


  In root kustomization.yaml file the below resources will be reference as follows :-

  apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# kubernetes resources to be managed by kustomize
resources:
  - db/
  - message-broker/
  - nginx/

   get pods -A
NAMESPACE     NAME                                     READY   STATUS      RESTARTS   AGE
kube-system   coredns-77ccd57875-4d6fg                 1/1     Running     0          5h47m
kube-system   local-path-provisioner-957fdf8bc-v9xkc   1/1     Running     0          5h47m
kube-system   helm-install-traefik-crd-4w8m5           0/1     Completed   0          5h47m
kube-system   helm-install-traefik-glrhj               0/1     Completed   1          5h47m
kube-system   svclb-traefik-c57a3fef-h54n4             2/2     Running     0          5h47m
kube-system   traefik-84745cf649-wg8cx                 1/1     Running     0          5h47m
kube-system   metrics-server-54dc485875-5tmzb          1/1     Running     0          5h47m
default       rabbitmq-deployment-57498fd79f-6jvds     1/1     Running     0          4m31s
default       nginx-deployment-545999b84c-h5f6p        1/1     Running     0          4m31s
default       nginx-deployment-545999b84c-9rnx8        1/1     Running     0          4m31s
default       rabbitmq-deployment-57498fd79f-7xqwl     1/1     Running     0          4m31s
default       nginx-deployment-545999b84c-rbkhs        1/1     Running     0          4m31s
default       db-deployment-7bdb76d77-zpxzl            1/1     Running     0          4m31s

How many pods were created in total?.

6
