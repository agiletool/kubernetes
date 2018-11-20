# What it is?
---


# 1. Install:
---

## Kops for AWS:
Guide: https://kubernetes.io/docs/setup/custom-cloud/kops/

### Install kubectl on your computer to controll:
Guide: https://kubernetes.io/docs/tasks/tools/install-kubectl/

On Ubuntu:
```
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

### Install kops:

On Linux:
```
wget https://github.com/kubernetes/kops/releases/download/1.10.0/kops-linux-amd64
chmod +x kops-linux-amd64
mv kops-linux-amd64 /usr/local/bin/kops
```

### Create a route53 domain for your cluster:

Example: **demo.magestore.com**

Using AWS CLI: ```aws route53 create-hosted-zone --name demo.magestore.com --caller-reference 1```

After that check ```dig NS demo.magestore.com``` you should see the 4 NS records that Route53 assigned your hosted zone.

Note that, if haven't been install aws cli install it first, then run ```aws configure``` to login with AWS Access ID and Key

### Create an S3 bucket to store your clusters state:

Using AWS CLI: ```aws s3 mb s3://clusters.demo.magestore.com```

### Build cluster configuration:

Using AWS CLI: ```kops create cluster --zones=us-east-1f useast1.demo.magestore.com```

This step will create configuration and store to aws s3.

Note: Guide for create cluster configuration from [template](https://github.com/kubernetes/kops/blob/master/docs/cluster_template.md)

### Create the cluster in AWS:

Using AWS CLI: ```kops update cluster useast1.demo.magestore.com --yes```

This step will create EC2 Instance, Auto Scaling Group.


# 2. Usage
---
