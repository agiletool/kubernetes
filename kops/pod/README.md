# How to run

Create pod:

```
kubectl create -f demo.yaml
```

Expose the pod:

```
kubectl expose pod <pod_name> --type=NodePort --port=80
```


# Other guide

## Creating an EBS volume [Docs](https://kubernetes.io/docs/concepts/storage/volumes/)
Before you can use an EBS volume with a Pod, you need to create it.

aws ec2 create-volume --availability-zone=eu-west-1a --size=10 --volume-type=gp2
Make sure the zone matches the zone you brought up your cluster in. (And also check that the size and EBS volume type are suitable for your use!)

## AWS EBS Example configuration
apiVersion: v1
kind: Pod
metadata:
  name: test-ebs
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-ebs
      name: test-volume
  volumes:
  - name: test-volume
    # This AWS EBS volume must already exist.
    awsElasticBlockStore:
      volumeID: <volume-id>
      fsType: ext4
      
