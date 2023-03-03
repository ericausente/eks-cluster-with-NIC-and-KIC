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

e.ausente@C02DR4L1MD6M .aws % brew install eksctl

Give 

eksctl create cluster \
  --version 1.26.2 \
  --region ap-southeast-1 \
  --nodes 3 \
  --name k8s-cluster-idfc
  
  The eksctl tool uses CloudFormation under the hood, creating one stack for the EKS master control plane and another stack for the worker nodes.
  
  See the logs in logs.md file. 
  
  
  
  
  It's hard to say exactly why a consultant might have said that NGINX is not a forward proxy product without more context, but it's possible that they were referring to NGINX's capabilities beyond simply functioning as a forward proxy.

While NGINX is certainly capable of acting as a forward proxy, it is a versatile web server and reverse proxy that can also be used for load balancing, content caching, and other purposes. In fact, many people use NGINX primarily as a reverse proxy rather than as a forward proxy.

It's possible that the consultant was simply pointing out that NGINX's capabilities go beyond what you might expect from a typical forward proxy product. However, without more context, it's hard to say for certain what the consultant meant. If you have any concerns or questions, you should ask the consultant directly for clarification.





### To create an Ingress Controller on Amazon Elastic Kubernetes Service (EKS), you can follow these steps:

Create an Amazon EKS cluster: If you don't already have an EKS cluster, you can create one using the Amazon EKS console, AWS CLI, or an infrastructure-as-code tool like Terraform.
Install and configure kubectl: kubectl is a command-line tool that you use to deploy and manage applications on Kubernetes clusters. You can download and install kubectl from the Kubernetes documentation.
Install and configure the AWS CLI: The AWS CLI is a command-line tool that you use to interact with Amazon Web Services. You can download and install the AWS CLI from the AWS documentation.
Install the NGINX Ingress Controller: You can install the NGINX Ingress Controller on your EKS cluster using a Helm chart. To do this, run the following commands in your terminal:

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx
```


These commands will add the NGINX Ingress Controller Helm chart repository, update your local repositories, and install the Ingress Controller on your EKS cluster.

Verify the installation: After installing the Ingress Controller, you can verify that it is running correctly by running the following command in your terminal:
csharp
Copy code
kubectl get pods -n ingress-nginx
This command will display the pods running in the ingress-nginx namespace, including the NGINX Ingress Controller pod.

That's it! You have now created an Ingress Controller on your EKS cluster using NGINX. You can now configure the Ingress Controller to route traffic to your applications running on the cluster.
