# Tworzenie klastra

Start docker engine

`minikube start --nodes 3`

`minikube dashboard`

`minikube addons enable metrics-server`

# Zarządzanie pojedynczym pod

`kubectl apply -f <config-file>`
`kubectl port-forward kuard 8080:8080`

Pokazać health checks i resource management

### Volumes

`docker exec -it <node-name> bash`
`echo "Hello world" > /var/lib/kuard/test.txt`

Sprawdzić `/data/test.txt` w UI

# ReplicaSet

### Manual

`kubectl apply -f 3-kuard-rs.yaml`

`kubectl scale replicasets kuard --replicas=1`

`kubectl delete replicasets kuard`

### Autoscale

`kubectl apply -f 2-kuard-pod-full.yaml`

`kubectl autoscale rs kuard --min=2 --max=5 --cpu-percent=50`

# Deployment

`kubectl apply -f 4-kuard-deployment.yaml`
`kubectl get replicasets -o wide`

W configu zmienić 
- `gcr.io/kuar-demo/kuard-amd64:green` na `gcr.io/kuar-demo/kuard-amd64:blue`
- Adnotacje change cause

`kubectl apply -f 4-kuard-deployment.yaml`
`kubectl get replicasets -o wide`

`kubectl rollout undo deployment kuard`

# Service discovery

```
kubectl create deployment alpaca-prod \
--image=gcr.io/kuar-demo/kuard-amd64:blue \
--port=8080
kubectl scale deployment alpaca-prod --replicas 3
kubectl expose deployment alpaca-prod
```

```
ALPACA_POD=$(kubectl get pods -l app=alpaca-prod \
  -o jsonpath='{.items[0].metadata.name}')
kubectl port-forward $ALPACA_POD 48858:8080
```

W kuard: DNS query `alpaca-prod`


### NodePort

```
kubectl delete service alpaca-prod
kubectl delete deployment alpaca-prod
```

```
kubectl create deployment alpaca-prod \
--image=gcr.io/kuar-demo/kuard-amd64:blue \
--port=8080
kubectl scale deployment alpaca-prod --replicas 3
kubectl expose deployment alpaca-prod --type=NodePort 
```

`minikube service alpaca-prod`


### Manual service discovery

`kubectl get pods -o wide --show-labels`
`kubectl get pods -o wide --selector=app=alpaca-prod`

# DaemonSet

`kubectl create -f 5-fluentd-daemonset.yaml`

`minikube node add`

`kubectl get pods -o wide`

# Job

### One shot

`kubectl apply -f 6-job-oneshot.yaml`

`kubectl logs oneshot`

`kubectl delete pod oneshot`


# StatefulSet (MongoDB)

# Zaglądanie do procesów w control plane

### etcd:

`docker exec -it minikube bash`
`cd /var/lib/minikube/certs/etcd`

```
sudo apt-get update -y
sudo apt-get install -y etcd-client
```

`ETCDCTL_API=3 etcdctl --cacert ca.crt --cert server.crt --key server.key --endpoints https://127.0.0.1:2379 get /registry/ --prefix --keys-only`

### kube-proxy:

`kubectl exec -it -n kube-system kube-proxy-##### -- iptables -L`

### kindnet:

`kubectl exec -it -n kube-system kindnet-##### -- iptables -L`