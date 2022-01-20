# - Kubernetes deployments/ configuration files 
    
        - ADDED

# - Jenkins Pipeline

        - ADDED FOR DEPLOYING REDIS HA CLUSTER

# - Report of what you did and how to test your setup

        - HOW AND TEST EXPLAIN ALL BELOW WITH SCREEN SHOTS

# - Explain how can we maitane the configurations and version them?

`Let's say in future we want to modify the redis docker image with this jenkins job we can modify the image as many time as we want coz we're adding tag in jenkin job and pushing image and using that tag in manifest so that how we can mantain and version them. By running the Jenkins job there's an option build with parameter where we can change the TAG and that value will be added to image and use in manifest. So that is how we can maintain versioning. And Configuration can be stored in any Version control.`


Using Kind for Kubernetes Cluster

Prerequisite - INSTALL DOCKER 

Using Kind for creating a kubernetes cluster

Documentation [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)

For installig [kubectl](https://kubernetes.io/docs/tasks/tools/)

For installing [Helm](https://helm.sh/docs/intro/install/)

For Installing Ingress Load balancer on Prem
   
    - kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/master/manifests/namespace.yaml

    - kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)" 

    - kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/master/manifests/metallb.yaml

    - kubectl get pods -n metallb-system --watch

    - docker network inspect -f '{{.IPAM.Config}}' kind
      ## With Above command you'll get the CIDR range, set that subnet in configmap

    - kubectl apply -f configmap.yaml
 
    - helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

    - helm repo add nginx-stable https://helm.nginx.com/stable

    - helm install ingress-nginx ingress-nginx/ingress-nginx -f ingress.yaml

Using [LENS KUBERNETES IDE](https://k8slens.dev/) for monitoring Kubernetes Cluster

**KUBE-CLUSTER OVERVIEW ON LENS**

![Kube-Cluster](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_250.png)

**INGRESS DEPLOYED**

![Ingress](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_251.png)


**Configure Jenkins Pipeline**

    - Clone code from repo
    - Set Build parameter for TAG 
    - Set Credentials for Docker Hub as we're pushing image to Docker HUB
    - Build image of redis HA Cluster with certain TAG Variable
    - Push image to docker hub
    - use this **REDIS HA IMAGE** from docker hub in kubernetes manifest
    - .kube directory for credentials already mounted inside jenkins image
    - Passing TAG variable in manifest for image
    - applying the manifest with kubectl 

**JENKINS JOB**

![JOB-1](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_252.png)

![JOB-2](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_253.png)

![JOB-3](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_254.png)

![JOB-4](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_255.png)

![JOB-5](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_259.png)

![JOB-6](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_258.png)


**IMAGE PUSHED TO DOCKER HUB**

![DOCKER-HUB](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_256.png)

And by deplying the **redis-ha-cluster.yaml** manifest following are the things which will be deployed

![REDIS-HA-CLUSTER-P](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_257.png)

![REDIS-HA-CLUSTER-D](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_260.png)

**REDIS CLUSTER EXPLANATION**

    - One Master Deployment
    - One Slave Deplyment
    - Three Sentinel Deployment
    - IF master goes down Sentinel will elect new master. Slave with most will become the master
    - Master will accepts all the writes and synchronously all to slaves.

**REDIS POD LOGS**

SENTINEL LOGS

![LOG-1](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_261.png)

SLAVE LOGS

![LOG-2](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_263.png)

MASTER LOGS

![LOG-2](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_264.png)


Deploying Prometheus and Garafana for monitoring metrics

    - helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    
    - helm repo add stable https://charts.helm.sh/stable 

    - helm repo update

    - helm install prometheus prometheus-community/kube-prometheus-stack

    - Once stack has been deployed for accessing Garafana we're using manifest which will create a service and ingress for this and made an host entry in your system for accessing it with name 
  
    - kubectl apply -f garafana.yaml
  
**Things Deployed for prometheus/Garafana Stack**

DEPLOYMENTS 

![PROM-1](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_265.png)

SERVICE INGRESS

![PROM-2](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_266.png)

After service is up we can access garafana dashboard by makng a host entry for the name which we set in our manifest 

![PROM-2](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_268.png)

DASHBOARD SCREEN-SHOT

![PROM-3](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_269.png)

![PROM-4](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_270.png)

![PROM-5](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_271.png)

![PROM-6](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_272.png)

![PROM-7](https://github.com/Muhammad-Irfan324/redis-cluster/blob/master/Selection_273.png)







