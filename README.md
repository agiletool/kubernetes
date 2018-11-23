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
pod  ' -- Run all containers created as servers (web services, api, etc...)
```
