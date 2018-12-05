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

* Create hard link like overlayFS:

```
find /var/log/ -type d -print0 | sed 's/\/var\/log\///g' | xargs -I % -0 mkdir -p % 2>/dev/null
find /var/log/ -type f -print0 | sed 's/\/var\/log\///g' | xargs -I % -0 ln -T /var/log/% %
```

Benefit: Modify or create a new file in destination directory is not create file in target directory, modify the hard link is modify to original file in target

* Force delete pod

``kubectl delete pod NAME --grace-period=0 --force``

