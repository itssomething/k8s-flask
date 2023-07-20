# How to build and deploy using eks, k8s, flask
## Install prerequisities
- aws cli https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html
- kubectl https://kubernetes.io/docs/tasks/tools/install-kubectl/
- kubectx (nice to have) https://stackoverflow.com/questions/69070582/how-can-i-install-kubectx-on-ubuntu-linux-20-04
- eksctl https://github.com/weaveworks/eksctl

You need to install those and login with aws cli correctly
## Creating a VPC
We will use this Cloudformation template, provide by aws: https://amazon-eks.s3.us-west-2.amazonaws.com/cloudformation/2020-06-10/amazon-eks-vpc-private-subnets.yaml

What this do?
- Create a VPC with CIDR address of 192.168.0.0/16
- Create two public subnets with CIDR blocks of 192.168.0.0/18 and 192.168.64.0/18
- Create two private subnets with CIDR blocks of 192.168.128.0/18 and 192.168.192.0/18

Output:

It will returns 1 security group for the vpc, 1 vpc id, 4 subnets ids

## Deploy EKS cluster
using `kubernetes/clusters.yaml`, replace vpc id with vpc id from above, replace 4 subnets ids with 4 subnet ids from above. You need to use correct private and public subnets, respectively

What it do:
- Create an EKS cluster named EKS-Demo-Cluster
- In node group, we create 3 workers with t2.micro instances with 2 public workers and 1 private worker

Run this command to create the EKS Cluster
```
eksctl create cluster -f kubernetes/clusters.yaml
```
Wait for a while for the cluster be created

## Deploy your application
- Use `kubernestes/deployment.yaml`
- Run `kubectl apply -f deployment.yaml`
