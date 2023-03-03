e.ausente@C02DR4L1MD6M .aws % eksctl create cluster
2023-03-03 10:21:00 [ℹ]  eksctl version 0.132.0-dev+15bffbb0d.2023-03-01T18:38:02Z
2023-03-03 10:21:00 [ℹ]  using region ap-southeast-1
2023-03-03 10:21:00 [ℹ]  setting availability zones to [ap-southeast-1b ap-southeast-1c ap-southeast-1a]
2023-03-03 10:21:00 [ℹ]  subnets for ap-southeast-1b - public:192.168.0.0/19 private:192.168.96.0/19
2023-03-03 10:21:00 [ℹ]  subnets for ap-southeast-1c - public:192.168.32.0/19 private:192.168.128.0/19
2023-03-03 10:21:00 [ℹ]  subnets for ap-southeast-1a - public:192.168.64.0/19 private:192.168.160.0/19
2023-03-03 10:21:00 [ℹ]  nodegroup "ng-9650d888" will use "" [AmazonLinux2/1.24]
2023-03-03 10:21:00 [ℹ]  using Kubernetes version 1.24
2023-03-03 10:21:00 [ℹ]  creating EKS cluster "scrumptious-painting-1677810060" in "ap-southeast-1" region with managed nodes
2023-03-03 10:21:00 [ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial managed nodegroup
2023-03-03 10:21:00 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=ap-southeast-1 --cluster=scrumptious-painting-1677810060'
2023-03-03 10:21:00 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "scrumptious-painting-1677810060" in "ap-southeast-1"
2023-03-03 10:21:00 [ℹ]  CloudWatch logging will not be enabled for cluster "scrumptious-painting-1677810060" in "ap-southeast-1"
2023-03-03 10:21:00 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=ap-southeast-1 --cluster=scrumptious-painting-1677810060'
2023-03-03 10:21:00 [ℹ]  
2 sequential tasks: { create cluster control plane "scrumptious-painting-1677810060", 
    2 sequential sub-tasks: { 
        wait for control plane to become ready,
        create managed nodegroup "ng-9650d888",
    } 
}
2023-03-03 10:21:00 [ℹ]  building cluster stack "eksctl-scrumptious-painting-1677810060-cluster"
^C
e.ausente@C02DR4L1MD6M .aws % eksctl create cluster \
  --version 1.25 \
  --region ap-southeast-1 \
  --nodes 3 \
  --name k8s-cluster-idfc
2023-03-03 10:21:12 [ℹ]  eksctl version 0.132.0-dev+15bffbb0d.2023-03-01T18:38:02Z
2023-03-03 10:21:12 [ℹ]  using region ap-southeast-1
2023-03-03 10:21:12 [ℹ]  setting availability zones to [ap-southeast-1c ap-southeast-1a ap-southeast-1b]
2023-03-03 10:21:12 [ℹ]  subnets for ap-southeast-1c - public:192.168.0.0/19 private:192.168.96.0/19
2023-03-03 10:21:12 [ℹ]  subnets for ap-southeast-1a - public:192.168.32.0/19 private:192.168.128.0/19
2023-03-03 10:21:12 [ℹ]  subnets for ap-southeast-1b - public:192.168.64.0/19 private:192.168.160.0/19
2023-03-03 10:21:12 [ℹ]  nodegroup "ng-7806d7b3" will use "" [AmazonLinux2/1.25]
2023-03-03 10:21:12 [ℹ]  using Kubernetes version 1.25
2023-03-03 10:21:12 [ℹ]  creating EKS cluster "k8s-cluster-idfc" in "ap-southeast-1" region with managed nodes
2023-03-03 10:21:12 [ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial managed nodegroup
2023-03-03 10:21:12 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=ap-southeast-1 --cluster=k8s-cluster-idfc'
2023-03-03 10:21:12 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "k8s-cluster-idfc" in "ap-southeast-1"
2023-03-03 10:21:12 [ℹ]  CloudWatch logging will not be enabled for cluster "k8s-cluster-idfc" in "ap-southeast-1"
2023-03-03 10:21:12 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=ap-southeast-1 --cluster=k8s-cluster-idfc'
2023-03-03 10:21:12 [ℹ]  
2 sequential tasks: { create cluster control plane "k8s-cluster-idfc", 
    2 sequential sub-tasks: { 
        wait for control plane to become ready,
        create managed nodegroup "ng-7806d7b3",
    } 
}
2023-03-03 10:21:12 [ℹ]  building cluster stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:21:13 [ℹ]  deploying stack "eksctl-k8s-cluster-idfc-cluster"
/var/folders/b9/0m630tp10_323rrxnc3gkg440000gp/T/TemporaryItems/NSIRD_screencaptureui_boEa4b/Screenshot\ 2023-03-03\ at\ 10.21.31\ AM.png 2023-03-03 10:21:43 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:22:13 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:23:13 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:24:13 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:25:13 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:26:13 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:27:14 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:28:14 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:29:14 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:30:14 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:31:14 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:32:14 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-cluster"
2023-03-03 10:34:16 [ℹ]  building managed nodegroup stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:34:16 [ℹ]  deploying stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:34:16 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:34:46 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:35:24 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:36:19 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:38:11 [ℹ]  waiting for CloudFormation stack "eksctl-k8s-cluster-idfc-nodegroup-ng-7806d7b3"
2023-03-03 10:38:11 [ℹ]  waiting for the control plane to become ready
2023-03-03 10:38:12 [✔]  saved kubeconfig as "/Users/e.ausente/.kube/config"
2023-03-03 10:38:12 [ℹ]  no tasks
2023-03-03 10:38:12 [✔]  all EKS cluster resources for "k8s-cluster-idfc" have been created
2023-03-03 10:38:13 [ℹ]  nodegroup "ng-7806d7b3" has 3 node(s)
2023-03-03 10:38:13 [ℹ]  node "ip-192-168-26-220.ap-southeast-1.compute.internal" is ready
2023-03-03 10:38:13 [ℹ]  node "ip-192-168-44-89.ap-southeast-1.compute.internal" is ready
2023-03-03 10:38:13 [ℹ]  node "ip-192-168-77-30.ap-southeast-1.compute.internal" is ready
2023-03-03 10:38:13 [ℹ]  waiting for at least 3 node(s) to become ready in "ng-7806d7b3"
2023-03-03 10:38:13 [ℹ]  nodegroup "ng-7806d7b3" has 3 node(s)
2023-03-03 10:38:13 [ℹ]  node "ip-192-168-26-220.ap-southeast-1.compute.internal" is ready
2023-03-03 10:38:13 [ℹ]  node "ip-192-168-44-89.ap-southeast-1.compute.internal" is ready
2023-03-03 10:38:13 [ℹ]  node "ip-192-168-77-30.ap-southeast-1.compute.internal" is ready
2023-03-03 10:38:13 [✖]  kubectl not found, v1.10.0 or newer is required
2023-03-03 10:38:13 [ℹ]  cluster should be functional despite missing (or misconfigured) client binaries
2023-03-03 10:38:13 [✔]  EKS cluster "k8s-cluster-idfc" in "ap-southeast-1" region is ready
