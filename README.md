# eks-cluster with NGINX Kubernetes Ingress Controller (Kubernetes project) and NGINX Plus Ingress Controller (from NGINX)

1. Plan your cluster architecture: Before you create an EKS cluster, you need to plan the architecture of your cluster. This includes deciding on the number and types of nodes, selecting the instance types, and designing your networking and security configuration.
2. Create a Virtual Private Cloud (VPC): EKS requires a VPC to be created for the cluster. You can create a new VPC for the cluster or use an existing one. If you create a new VPC, you will need to configure the subnets, routing tables, and security groups.
3. Create an Amazon EKS cluster: You can create an EKS cluster using the AWS Management Console, AWS CLI, or an infrastructure-as-code tool like Terraform. During the creation process, you will need to specify the cluster name, region, VPC, and other configuration details.

In my case I have configured using AWS CLI credentials on my MacBook. 

```
brew install aws-cli
aws --version
```

The ~/.aws directory contains my  AWS configuration and credentials files, which are used by the AWS CLI to authenticate and authorize your requests to AWS services.

The config file contains my  AWS CLI configuration settings, such as the default region and output format. The credentials file contains my AWS access key ID and secret access key (and token), which are used to authenticate your requests to AWS services.

By running the 
```
aws configure
````
command, you can set up your AWS CLI credentials and configure your AWS CLI settings, including your default region, output format, and more.

```
% tree /Users/e.ausente/.aws 
/Users/e.ausente/.aws
├── config
└── credentials

% cat ~/.aws/credentials 
[default]
aws_access_key_id = XXXXXXXX
aws_secret_access_key = XXXXXXXX
aws_session_token = XXXXXXXXXXXXXXXXXXXXXXXX

% cat ~/.aws/config     
[default]
region = ap-southeast-1
output = json

````

Install eksctl: Use Homebrew to install eksctl on your MacBook by running the following command in your terminal:

```
% brew install eksctl
```
Create an EKS cluster: Use the eksctl create cluster command to create an EKS cluster. Replace k8s-cluster-eric with a name for your cluster, and ap-southeast-1 with the region where you want to create the cluster. You can also adjust the number of worker nodes by changing the --nodes parameter.

```
eksctl create cluster \
  --version 1.26.2 \
  --region ap-southeast-1 \
  --nodes 3 \
  --name k8s-cluster-eric
``` 
 
View the CloudFormation logs: After running the eksctl command, it will create two CloudFormation stacks for your EKS cluster: one for the master control plane and another for the worker nodes. You can view the CloudFormation logs by running the following command and redirecting the output to a file:

```
% eksctl utils describe-stacks --region ap-southeast-1 --cluster k8s-cluster-eric > logs.md
```
See the sample logs in logs.md file. 
  
  
Update the kubeconfig file: Use the AWS CLI to update the kubeconfig file for your EKS cluster. Replace k8s-cluster-eric with the name of your cluster.

  ```
  % aws eks update-kubeconfig --name k8s-cluster-eric
Added new context arn:aws:eks:ap-southeast-1:150380270330:cluster/k8s-cluster-eric to /Users/e.ausente/.kube/config
  ```
  
Install kubectl: Use Homebrew to install kubectl on your MacBook by running the following command in your terminal:
```
brew install kubectl
```

The kubectl cluster-info command is used to display the Kubernetes control plane and CoreDNS endpoints for your EKS cluster.

```
% kubectl cluster-info
Kubernetes control plane is running at https://9AA71B002C155619411D5CF2F4E607E2.gr7.ap-southeast-1.eks.amazonaws.com
CoreDNS is running at https://9AA71B002C155619411D5CF2F4E607E2.gr7.ap-southeast-1.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

% kubectl get nodes -o wide
NAME                                                STATUS   ROLES    AGE   VERSION               INTERNAL-IP      EXTERNAL-IP     OS-IMAGE         KERNEL-VERSION                  CONTAINER-RUNTIME
ip-192-168-26-220.ap-southeast-1.compute.internal   Ready    <none>   19m   v1.25.6-eks-48e63af   192.168.26.220   13.214.204.23   Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   containerd://1.6.6
ip-192-168-44-89.ap-southeast-1.compute.internal    Ready    <none>   19m   v1.25.6-eks-48e63af   192.168.44.89    52.77.211.225   Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   containerd://1.6.6
ip-192-168-77-30.ap-southeast-1.compute.internal    Ready    <none>   19m   v1.25.6-eks-48e63af   192.168.77.30    13.229.114.22   Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   containerd://1.6.6

% eksctl get cluster
NAME				REGION		EKSCTL CREATED
k8s-cluster-eric		ap-southeast-1	True
```

Install the NGINX Ingress Controller: You can install the NGINX Ingress Controller on your EKS cluster using a Helm chart. To do this, run the following commands in your terminal:

```
brew install helm
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx
```

These commands will add the NGINX Ingress Controller Helm chart repository, update your local repositories, and install the Ingress Controller on your EKS cluster.

Verify the installation: After installing the Ingress Controller, you can verify that it is running correctly by running the following command in your terminal:
 
```
kubectl get pods -n ingress-nginx
```

This command will display the pods running in the ingress-nginx namespace, including the NGINX Ingress Controller pod.
It did create a Loadbalancer type of service for my IC: 

```
 % kubectl get pods 
NAME                                        READY   STATUS    RESTARTS   AGE
ingress-nginx-controller-6b94c75599-7sjq9   1/1     Running   0          38s
nginx                                       1/1     Running   0          104m
  
% kubectl get svc
NAME                                 TYPE           CLUSTER-IP      EXTERNAL-IP                                                                  PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.100.245.69   acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com   80:31594/TCP,443:31068/TCP   49s
ingress-nginx-controller-admission   ClusterIP      10.100.123.13   <none>                                                                       443/TCP                      49s
kubernetes                           ClusterIP      10.100.0.1      <none>                                                                       
```

To inspect the YAML files created by Helm, you can use the helm get command with the --output flag to output the chart's manifests. Here's an example command (Replace RELEASE_NAME with the name of the Helm release. In my case it is ingres-nginx):

```
 % helm get manifest ingress-nginx >> helm-ingress-manifest.yaml
  
% more helm-ingress-manifest.yaml 
---
# Source: ingress-nginx/templates/controller-serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.5.2
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/version: "1.6.4"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: ingress-nginx
  namespace: default
automountServiceAccountToken: true
---
# Source: ingress-nginx/templates/controller-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  labels:
    helm.sh/chart: ingress-nginx-4.5.2
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/version: "1.6.4"
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/component: controller
  name: ingress-nginx-controller
  namespace: default
data:
  allow-snippet-annotations: "true"
---
# Source: ingress-nginx/templates/clusterrole.yaml
```

Open a text editor and create a new file called ingress.yaml.

This is just a sample manifest: 

```
% cat ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  creationTimestamp: null
  name: ingress
spec:
  ingressClassName: nginx
  rules:
  - host: acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com
    http:
      paths:
      - backend:
          service:
            name: nginx-dep
            port:
              number: 80
        path: /
        pathType: Prefix
status:
  loadBalancer: {}
```
Replace acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com with the hostname or IP address of your EKS cluster's Elastic Load Balancer.
Replace nginx-dep with the name of the Kubernetes service that you want to expose through the Ingress.
Save the ingress.yaml file and exit the text editor.
Apply the ingress.yaml file to your EKS cluster using the following command:

```
  % kubectl apply -f ingress.yaml
```
  
Verify that the Ingress was created successfully by running the following command:
```
  % kubectl get ingress
```

  Verify that the Ingress is routing traffic correctly by accessing the URL that you specified in the Ingress rule. In the example YAML code above, you would access http://acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com/nginx to access the nginx-dep service.


Added Step of creation of a CNAME record in GoDaddy as I want to make my Kubernetes Ingress URL easier to read:


1. Log in to your GoDaddy account and navigate to the "DNS Management" page for your domain.
2. Locate the "CNAME (Alias)" section and click the "Add" button.
3. In the "Alias" field, enter the name of the subdomain you want to use for the CNAME record (e.g., "nginxkic").
4. In the "Points to" field, enter the hostname of your Kubernetes cluster's load balancer (i.e., acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com).
5. Set the "TTL" value as desired.
6. Click the "Save" button to create the CNAME record.


Once the CNAME record has been created, I can use the subdomain nginxkic.kushikimi.xyz to access my Kubernetes Ingress instead of the long and complex load balancer URL. This makes it easier to remember and share the Ingress URL with others.

Change the host of your yaml file: 
```
% cat ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  creationTimestamp: null
  name: ingress
spec:
  ingressClassName: nginx
  rules:
  - host: nginxkic.kushikimi.xyz
    http:
      paths:
      - backend:
          service:
            name: nginx-dep
            port:
              number: 80
        path: /
        pathType: Prefix
status:
  loadBalancer: {}

% kubectl get ingress
NAME      CLASS   HOSTS                    ADDRESS                                                                      PORTS   AGE
ingress   nginx   nginxkic.kushikimi.xyz   acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com   80      3h8m
```

To verify that the hostname is working correctly, I can try accessing http://nginxkic.kushikimi.xyz from the web browser. If everything is configured correctly, you should see the website or application that is being served by the Ingress.


## Additional Challenge
## Now let's spin up a new Ingress Controller built on NGINX Plus

# Installation of k8s-ingress with Helm
https://docs.nginx.com/nginx-ingress-ontroller/installation/installation-with-helm/

Go back to your home directory: 
```
$ cd ~
```

Clone the Ingress Controller repo:
```
$ git clone https://github.com/nginxinc/kubernetes-ingress.git --branch v2.3.0
```

Change your working directory to /deployments/helm-chart:
```
$ cd kubernetes-ingress/deployments/helm-chart
```

# Adding the Helm Repository
This step is required if you’re installing the chart via the helm repository.
```
$ helm repo add nginx-stable https://helm.nginx.com/stable
$ helm repo update
% helm repo list
NAME         	URL                                       
ingress-nginx	https://kubernetes.github.io/ingress-nginx
nginx-stable 	https://helm.nginx.com/stable       -------------->>>> 
```

# Using the NGINX IC Plus JWT token in a Docker Config Secret

Purpose: NGINX Plus Ingress Controller image from the F5 Docker registry in your Kubernetes cluster by using your NGINX Ingress Controller subscription JWT token 
Refer to https://docs.nginx.com/nginx-ingress-controller/installation/using-the-jwt-token-docker-secret/

1. Create a docker-registry secret on the cluster using the JWT token as the username, and none for password (password is unused). The name of the docker server is private-registry.nginx.com. Optionally namespace the secret.

Make sure to save your nginx-repo.jwt in /tmp/ directory. 

```
kubectl create secret docker-registry regcred --docker-server=private-registry.nginx.com --docker-username=$(cat /tmp/nginx-repo.jwt) --docker-password=none
```

Confirm the details of the created secret by running:
```
kubectl get secret regcred --output=yaml
```

Go back to your home directory: 
```
cd ~
```

Install the NGINX Plus Ingress Controller using Helm and specify the new ingress class nginx-plus using the --set controller.ingressClass=nginx-plus flag.

The rest of the command sets various configuration options for the Ingress Controller, such as the image repository, NGINX Plus features, and service settings.

By setting controller.nginxplus=true, you are enabling NGINX Plus features like dynamic reconfiguration, session persistence, and advanced traffic management.

By setting controller.appprotect.enable=true, you are enabling the NGINX App Protect module for application security and protection.

The controller.service.type=LoadBalancer flag creates an AWS Elastic Load Balancer to route traffic to your Ingress Controller, and controller.service.externalTrafficPolicy=Cluster ensures that traffic is routed to the Kubernetes nodes running your Ingress Controller pods instead of being directly routed to the pods.

By setting controller.ingressClass=nginx-plus, you have specified the new ingress class that you created earlier and can now use this class to route traffic to your Kubernetes services and applications using the NGINX Plus Ingress Controller.
  
  Command:

  ```
helm install ingress-plus nginx-stable/nginx-ingress --set controller.ingressClass=nginx-plus --set controller.image.repository=private-registry.nginx.com/nginx-ic-nap/nginx-plus-ingress --set controller.nginxplus=true --set controller.appprotect.enable=true --set controller.image.tag=3.0.1 --set controller.serviceAccount.imagePullSecretName=regcred --set controller.service.type=LoadBalancer --set controller.service.httpsPort.nodePort=32137 --set controller.replicaCount=1 --set controller.kind=deployment --set prometheus.create=true --set controller.readyStatus.initialDelaySeconds=30 --set controller.service.externalTrafficPolicy=Cluster
```

OUTPUT: 
```
% helm install ingress-plus nginx-stable/nginx-ingress --set controller.ingressClass=nginx-plus --set controller.image.repository=private-registry.nginx.com/nginx-ic-nap/nginx-plus-ingress --set controller.nginxplus=true --set controller.appprotect.enable=true --set controller.image.tag=3.0.1 --set controller.serviceAccount.imagePullSecretName=regcred --set controller.service.type=LoadBalancer --set controller.service.httpsPort.nodePort=32137 --set controller.replicaCount=1 --set controller.kind=deployment --set prometheus.create=true --set controller.readyStatus.initialDelaySeconds=30 --set controller.service.externalTrafficPolicy=Cluster

NAME: ingress-plus
LAST DEPLOYED: Fri Mar  3 17:17:19 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The NGINX Ingress Controller has been installed.

 e.ausente@C02DR4L1MD6M eks-play % kubectl get ingressclass
NAME         CONTROLLER                     PARAMETERS   AGE
nginx        k8s.io/ingress-nginx           <none>       4h24m
nginx-plus   nginx.org/ingress-controller   <none>       9s

% kubectl get svc
NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP                                                                   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.100.245.69    acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com    80:31594/TCP,443:31068/TCP   4h25m
ingress-nginx-controller-admission   ClusterIP      10.100.123.13    <none>                                                                        443/TCP                      4h25m
ingress-plus-nginx-ingress           LoadBalancer   10.100.249.211   a31ce143913004c26a8386cbbe71434b-176863799.ap-southeast-1.elb.amazonaws.com   80:30711/TCP,443:31900/TCP   63s
kubernetes                           ClusterIP      10.100.0.1       <none>                                                                        443/TCP                      6h51m
nginx-dep                            ClusterIP      10.100.208.228   <none>                                                                        80/TCP  
```

Now let's create an app specifically for our nginx-plus to serve

```
kubectl create deploy kopi-teh --image=tsanghan/kopi-teh:v1 --replicas=2 --port 8000
kubectl expose deployment kopi-teh --port=80 --target-port=8000
kubectl get svc -o wide
kubectl apply -f ingress-plus.yaml

% cat ingress-plus.yaml 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  creationTimestamp: null
  name: ingress-plus
spec:
  ingressClassName: nginx-plus
  rules:
  - host: nginxplusnic.kushikimi.xyz 
    http:
      paths:
      - backend:
          service:
            name: kopi-teh
            port:
              number: 80
        path: /
        pathType: Prefix
status:
  loadBalancer: {}
```

```
% kubectl get ingress
NAME           CLASS        HOSTS                        ADDRESS                                                                       PORTS   AGE
ingress        nginx        nginxkic.kushikimi.xyz       acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com    80      3h59m
ingress-plus   nginx-plus   nginxplusnic.kushikimi.xyz   a31ce143913004c26a8386cbbe71434b-176863799.ap-southeast-1.elb.amazonaws.com   80      4s
```

Map a CNAME record for the NGINX Plus Ingress Controller using GoDaddy:
```
Log in to your GoDaddy account and navigate to the "DNS Management" page for your domain.
Locate the "CNAME (Alias)" section and click the "Add" button.
In the "Alias" field, enter the name of the subdomain you want to use for the CNAME record. For example, you could enter "nginxplusnic" to create a subdomain called nginxplusnic.yourdomain.com.
In the "Points to" field, enter the hostname of your Kubernetes cluster's load balancer. For example, if your load balancer URL is a31ce143913004c26a8386cbbe71434b-176863799.ap-southeast-1.elb.amazonaws.com, you would enter a31ce143913004c26a8386cbbe71434b-176863799.ap-southeast-1.elb.amazonaws.com as the value.
Set the "TTL" (Time to Live) value as desired. This determines how long the DNS resolver will cache the CNAME record before checking for updates.
Click the "Save" button to create the CNAME record.
```
  
Once the CNAME record has been created, you can use the subdomain nginxplusnic.yourdomain.com to access your NGINX Plus Ingress Controller instead of the long and complex load balancer URL.

http://nginxplusnic.kushikimi.xyz
