# create cluster
```
kind create cluster --config=- <<EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: ambient
nodes:
- role: control-plane
- role: worker
- role: worker
EOF
```

# setup lb

https://kind.sigs.k8s.io/docs/user/loadbalancer/


```
% docker network inspect -f '{{.IPAM.Config}}' kind
[{172.20.0.0/16  172.20.0.1 map[]} {fc00:f853:ccd:e793::/64   map[]}]
```

edit metallb-config and apply manifest.
```
kubectl apply -f metallb-config.yaml
```
