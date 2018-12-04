# kubernetes
Kubernetes for cloud


# How to this workflow

```
docker -- Build docker images and services (web services, api, etc..)
     |
     |
    \|/
kpos ' -- Install & create EC2 clusters to run kubernetes with auto scaling and load balancing
     |
     |
    \|/
demo(pod, statefulset)  -- Run all containers created as servers (web services, api, etc...)
```

# Run

After create cluster complete run following command

```
kubectl create -f demo/aws-ingress.yaml -f demo/nfs-volume.yaml -f demo/secret.yaml -f demo/ingress-service.yaml
```

wait for complete and run:

```
kubectl create -f demo/jobs-1.yaml
```

wait for complete and run:

```
kubectl create -f demo/jobs-2.yaml
```

wait for complete and run:

```
kubectl create -f demo/statefulset.yaml
```
