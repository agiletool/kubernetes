# How to run

Create pod:

```
kubectl create -f demo.yaml
```

Expose the pod:

```
kubectl expose pod <pod_name> --type=NodePort --port=80
```
