Usage:
---

* Run ```mandatory.yaml``` before ```demo-statefulset.yaml```


***Note:***

* Create EFS volume:

https://github.com/kubernetes-incubator/external-storage/tree/master/aws/efs

https://github.com/kubernetes-incubator/external-storage/tree/master/nfs/deploy/kubernetes

* Create EBS volume:

```
aws ec2 create-volume --size 10 --region us-east-1 --availability-zone us-east-1f --volume-type gp2 --tag-specifications 'ResourceType=volume,Tags=[{Key=KubernetesCluster,Value=useast1.demo.magestore.com}]'
```
Node: add a tag to volume with key ``KubernetesCluster`` value ``<clustername-here>``, for example ``useast1.demo.magestore.com``.

* Create TLS:

```
kubectl create secret tls custom-tls-cert --key /path/to/tls.key --cert /path/to/tls.crt
```
