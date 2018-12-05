# What it is?


# 1. Install:


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

### Install kops on your computer:

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

```
export KOPS_STATE_STORE="s3://clusters.demo.magestore.com"
```

### Build cluster configuration:

Create cluster with more than options:

```
kops create cluster useast1.demo.magestore.com \
  --state "s3://clusters.demo.magestore.com" \
  --node-count 1 \
  --zones us-east-1f \
  --vpc vpc-a7223ede \
  --subnets subnet-ea8818e5 \
  --node-size m4.large \
  --master-size m4.large \
  --master-zones us-east-1f \
  --master-volume-size 20 \
  --node-volume-size 20 \
  --yes
```

Optional: create cluster by yaml config (kops get cluster $NAME -o yaml), use ``kops create -f FILENAME``

```
apiVersion: kops/v1alpha2
kind: Cluster
metadata:
  creationTimestamp: null
  name: useast1.demo.magestore.com
spec:
  api:
    dns: {}
  authorization:
    rbac: {}
  channel: stable
  cloudProvider: aws
  configBase: s3://clusters.demo.magestore.com/useast2.demo.magestore.com
  etcdClusters:
  - etcdMembers:
    - instanceGroup: master-us-east-1f
      name: f
    name: main
  - etcdMembers:
    - instanceGroup: master-us-east-1f
      name: f
    name: events
  iam:
    allowContainerRegistry: true
    legacy: false
  kubernetesApiAccess:
  - 0.0.0.0/0
  kubernetesVersion: 1.10.6
  masterPublicName: api.useast1.demo.magestore.com
  networkCIDR: 172.31.0.0/16
  networkID: vpc-a7223ede
  networking:
    kubenet: {}
  nonMasqueradeCIDR: 100.64.0.0/10
  sshAccess:
  - 0.0.0.0/0
  subnets:
  - cidr: 172.31.96.0/20
    id: subnet-ea8818e5
    name: us-east-1f
    type: Public
    zone: us-east-1f
  topology:
    dns:
      type: Public
    masters: public
    nodes: public

---

apiVersion: kops/v1alpha2
kind: InstanceGroup
metadata:
  creationTimestamp: null
  labels:
    kops.k8s.io/cluster: useast1.demo.magestore.com
  name: master-us-east-1f
spec:
  image: kope.io/k8s-1.10-debian-jessie-amd64-hvm-ebs-2018-08-17
  machineType: m4.large
  maxSize: 1
  minSize: 1
  nodeLabels:
    kops.k8s.io/instancegroup: master-us-east-1f
  role: Master
  rootVolumeSize: 20
  subnets:
  - us-east-1f

---

apiVersion: kops/v1alpha2
kind: InstanceGroup
metadata:
  creationTimestamp: null
  labels:
    kops.k8s.io/cluster: useast1.demo.magestore.com
  name: nodes
spec:
  image: kope.io/k8s-1.10-debian-jessie-amd64-hvm-ebs-2018-08-17
  machineType: m4.large
  maxSize: 5
  minSize: 1
  nodeLabels:
    kops.k8s.io/instancegroup: nodes
  role: Node
  rootVolumeSize: 20
  subnets:
  - us-east-1f
```

Note: ```--subnets``` must created in AWS PVC

Note: [edit guide](https://github.com/kubernetes/kops/blob/master/docs/cli/kops_edit_cluster.md) if needed
Use ```kops edit instancegroup nodes --name useast1.demo.magestore.com``` to edit node-count, etc

Create SSH access key if not created:

```
ssh-keygen -t rsa -b 4096 -C "kubernetes-access"
```

Or we can edit file to add existed SSH Key to ```~/.ssh/id_rsa.pub```

Add SSH Key:

```
kops create secret --name useast1.demo.magestore.com sshpublickey admin -i ~/.ssh/id_rsa.pub
```

This step will create configuration and store to aws s3.

Note: Guide for create cluster configuration from [template](https://github.com/kubernetes/kops/blob/master/docs/cluster_template.md)

### Create the cluster in AWS:

```kops update cluster useast1.demo.magestore.com --yes```

This step will create EC2 Instance, Auto Scaling Group.

Result:

```
Cluster is starting.  It should be ready in a few minutes.

Suggestions:
 * validate cluster: kops validate cluster
 * list nodes: kubectl get nodes --show-labels
 * ssh to the master: ssh -i ~/.ssh/id_rsa admin@api.useast1.demo.magestore.com
 * the admin user is specific to Debian. If not using Debian please use the appropriate user based on your OS.
 * read about installing addons at: https://github.com/kubernetes/kops/blob/master/docs/addons.md.
```

# 2. Guides & Docs

## Kops:

* Docs: https://github.com/kubernetes/kops/tree/master/docs

Clean: ```kops delete cluster useast1.demo.magestore.com --state='s3://clusters.demo.magestore.com' --yes```

# 3. Operations

## Deploy app


# 4. Other:

- http://blog.arungupta.me/stateful-containers-kubernetes-amazon-ebs/
