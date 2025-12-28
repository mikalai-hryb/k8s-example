# k8s-example

You can find a few commands to create 3-nodes cluster (2 worker nodes) with the help of minikube.

```bash
minikube start --nodes 3 --disk-size 50000mb --profile k8s-example
minikube profile list

kubectl get nodes
kubectl get nodes --show-labels

kubectl label node k8s-example-m02 node-role.kubernetes.io/worker=worker && \
kubectl label node k8s-example-m02 role=worker && \
kubectl label node k8s-example-m03 node-role.kubernetes.io/worker=worker && \
kubectl label node k8s-example-m03 role=worker

minikube stop --profile k8s-example && \
minikube delete --profile k8s-example
```
