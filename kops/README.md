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

### Create a route53 domain for your cluster

Example: **demo.magestore.com**

Create via AWS CLI: ```aws route53 create-hosted-zone --name demo.magestore.com --caller-reference 1```

After that check ```dig NS demo.magestore.com``` you should see the 4 NS records that Route53 assigned your hosted zone.


# 2. Usage
---
