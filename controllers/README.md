Usage:
---

* Run mandatory.yaml before demo-statefulset.yaml


Note:

Create EBS volume:

```
aws ec2 create-volume --size 10 --region us-east-1 --availability-zone us-east-1f --volume-type gp2 --tag-specifications 'ResourceType=volume,Tags=[{Key=KubernetesCluster,Value=useast1.demo.magestore.com}]'
```

With ```useast1.demo.magestore.com``` is cluster name
