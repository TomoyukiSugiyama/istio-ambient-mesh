# deploy deployment and service
```bash
kubectl apply -f sample-deployment-blue.yaml
kubectl apply -f sample-deployment-green.yaml
kubectl apply -f sample-service-blue.yaml
kubectl apply -f sample-service-green.yaml
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
kubectl delete -f sample-service-blue.yaml
kubectl delete -f sample-service-green.yaml
kubectl delete -f sample-deployment-blue.yaml
kubectl delete -f sample-deployment-green.yaml
```