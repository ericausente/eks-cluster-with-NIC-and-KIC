# sources 
https://www.youtube.com/watch?v=rJQb_whepEY
https://seed-platform.org/code_documentation/2.15.1/kubernetes_deployment.html


brew install aws-cli
aws --version
aws configure

# eks-cluster

do aws configure to make sure
e.ausente@C02DR4L1MD6M ~ % tree /Users/e.ausente/.aws 
/Users/e.ausente/.aws
├── config
└── credentials

1 directory, 2 files

----


Install eksctl

% brew install eksctl

Give 

eksctl create cluster \
  --version 1.26.2 \
  --region ap-southeast-1 \
  --nodes 3 \
  --name k8s-cluster-idfc
  
  The eksctl tool uses CloudFormation under the hood, creating one stack for the EKS master control plane and another stack for the worker nodes.
  
  See the logs in logs.md file. 
  
  
Let's update the kube config with AWS CLI
The AWS CLI is used to update the Kubernetes configuration file for an Amazon Elastic Kubernetes Service (EKS) cluster.
  ```
  % aws eks update-kubeconfig --name k8s-cluster-idfc
Added new context arn:aws:eks:ap-southeast-1:150380270330:cluster/k8s-cluster-idfc to /Users/e.ausente/.kube/config
  ```
  
  
Download and install Kubectl:
```
brew install kubectl
```

```
% kubectl cluster-info
Kubernetes control plane is running at https://9AA71B002C155619411D5CF2F4E607E2.gr7.ap-southeast-1.eks.amazonaws.com
CoreDNS is running at https://9AA71B002C155619411D5CF2F4E607E2.gr7.ap-southeast-1.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

```
e.ausente@C02DR4L1MD6M .aws % kubectl get nodes -o wide
NAME                                                STATUS   ROLES    AGE   VERSION               INTERNAL-IP      EXTERNAL-IP     OS-IMAGE         KERNEL-VERSION                  CONTAINER-RUNTIME
ip-192-168-26-220.ap-southeast-1.compute.internal   Ready    <none>   19m   v1.25.6-eks-48e63af   192.168.26.220   13.214.204.23   Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   containerd://1.6.6
ip-192-168-44-89.ap-southeast-1.compute.internal    Ready    <none>   19m   v1.25.6-eks-48e63af   192.168.44.89    52.77.211.225   Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   containerd://1.6.6
ip-192-168-77-30.ap-southeast-1.compute.internal    Ready    <none>   19m   v1.25.6-eks-48e63af   192.168.77.30    13.229.114.22   Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   containerd://1.6.6
```
```
 eksctl get cluster
NAME				REGION		EKSCTL CREATED
k8s-cluster-idfc		ap-southeast-1	True
scrumptious-painting-1677810060	ap-southeast-1	True
```

brew install helm

Install the NGINX Ingress Controller: You can install the NGINX Ingress Controller on your EKS cluster using a Helm chart. To do this, run the following commands in your terminal:

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx
```


These commands will add the NGINX Ingress Controller Helm chart repository, update your local repositories, and install the Ingress Controller on your EKS cluster.

Verify the installation: After installing the Ingress Controller, you can verify that it is running correctly by running the following command in your terminal:
kubectl get pods -n ingress-nginx
This command will display the pods running in the ingress-nginx namespace, including the NGINX Ingress Controller pod.


It did create a Loadbalancer type of service for my IC: 

```
 % kubectl get pods 
NAME                                        READY   STATUS    RESTARTS   AGE
ingress-nginx-controller-6b94c75599-7sjq9   1/1     Running   0          38s
nginx                                       1/1     Running   0          104m
e.ausente@C02DR4L1MD6M ~ % kubectl get svc
NAME                                 TYPE           CLUSTER-IP      EXTERNAL-IP                                                                  PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.100.245.69   acf68105edd024bfe9fb72111124bfaf-32909998.ap-southeast-1.elb.amazonaws.com   80:31594/TCP,443:31068/TCP   49s
ingress-nginx-controller-admission   ClusterIP      10.100.123.13   <none>                                                                       443/TCP                      49s
kubernetes                           ClusterIP      10.100.0.1      <none>                                                                       
```

To inspect the YAML files created by Helm, you can use the helm get command with the --output flag to output the chart's manifests. Here's an example command (Replace RELEASE_NAME with the name of the Helm release. In my case it is ingres-nginx):

```
 % helm get manifest ingress-nginx >> helm-ingress-manifest.yaml
e.ausente@C02DR4L1MD6M ~ % more helm-ingress-manifest.yaml 
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

I think let's follow this: 
https://docs.primehub.io/docs/3.7/getting_started/kubernetes_on_eks


Don't forget 

```
### To create an Ingress Controller on Amazon Elastic Kubernetes Service (EKS), you can follow these steps:

Create an Amazon EKS cluster: If you don't already have an EKS cluster, you can create one using the Amazon EKS console, AWS CLI, or an infrastructure-as-code tool like Terraform.
Install and configure kubectl: kubectl is a command-line tool that you use to deploy and manage applications on Kubernetes clusters. You can download and install kubectl from the Kubernetes documentation.
Install and configure the AWS CLI: The AWS CLI is a command-line tool that you use to interact with Amazon Web Services. You can download and install the AWS CLI from the AWS documentation.


That's it! You have now created an Ingress Controller on your EKS cluster using NGINX. You can now configure the Ingress Controller to route traffic to your applications running on the cluster.
```

!!!
Metrics Server - show them the resourece usage between the KIC and NIC, as the former uses lua in their configuration
