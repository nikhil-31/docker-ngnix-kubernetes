# AWS Cloudformation commands

1. Create stack

```
aws cloudformation create-stack  --stack-name myFirstTest --region ap-south-1 --template-body file://testcfn.yml
```

2. Update stack

```
aws cloudformation update-stack  --stack-name myFirstTest --region us-east-1 --template-body file://testcfn.yml
```

3. Describe stack

```
aws cloudformation describe-stacks --stack-name myFirstTest
```

### Commands for creating EKS cluster on AWS

#### Commands must be run in the same order, as shown below,

1. Creating the iam role, network, cluster
```
aws cloudformation create-stack --stack-name eks-network --region us-east-1 --template-body file://cluster.yml --parameters file://network-params.json --capabilities CAPABILITY_NAMED_IAM
```

2. Creating the worker node group

```
aws cloudformation create-stack --stack-name worker-nodes --region us-east-1 --template-body file://worker-nodes.yml --parameters file://worker-nodes-params.json --capabilities CAPABILITY_NAMED_IAM
```

## Connecting to cluster

```
aws eks --region us-east-1 update-kubeconfig --name cfn-cluster
```