
# Minikube Setup

```bash
minikube start \
    --alsologtostderr \
    --memory 4G \
    --vm-driver virtualbox  \
    --host-only-cidr=10.225.2.1/24 \
    --docker-opt bip=10.224.0.1/16 \
    --extra-config kubeadm.pod-network-cidr=10.226.0.0/16
```

# Example Service on Minikube

See https://kubernetes.io/docs/tutorials/hello-minikube/

```bash
kc apply -f 1-service.yaml
kc apply -f 2-deployment.yaml
```

or

```bash
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

Wait for the Pods to start...

```bash
$ kubectl get pods -w
NAME                          READY   STATUS              RESTARTS   AGE
hello-node-7676b5fb8d-5lb8f   0/1     ContainerCreating   0          40s
hello-node-7676b5fb8d-5lb8f   1/1     Running             0          3m24s
```

...then start the browser:

```bash
$ minikube service hello-node
|-----------|------------|-------------|---------------------------|
| NAMESPACE |    NAME    | TARGET PORT |            URL            |
|-----------|------------|-------------|---------------------------|
| default   | hello-node |             | http://10.225.2.100:32456 |
|-----------|------------|-------------|---------------------------|
🎉  Opening service default/hello-node in default browser...
```