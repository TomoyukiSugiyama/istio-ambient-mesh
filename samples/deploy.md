# deploy deployment and service
```bash
kubectl apply -f sample-deployment.yaml
kubectl apply -f sample-service.yaml
```

# deploy gateway and httpRoute
```bash
kubectl apply -f sample-gateway.yaml
kubectl apply -f sample-httproute.yaml
```

# clean
```bash
kubectl delete -f sample-httproute.yaml
kubectl delete -f sample-gateway.yaml
kubectl delete -f sample-service.yaml
kubectl delete -f sample-deployment.yaml
```