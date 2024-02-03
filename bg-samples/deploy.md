# deploy deployment and service
```bash
helm template --set color=blue helm/app | kubectl apply -f -
helm template --set color=green helm/app | kubectl apply -f -
```

# deploy gateway and httpRoute
```bash
helm template helm/gateway | kubectl apply -f -
helm template helm/route | kubectl apply -f -
```

# clean
```bash
helm template helm/route | kubectl delete -f -
helm template helm/gateway | kubectl delete -f -
helm template --set color=green helm/app | kubectl delete -f -
helm template --set color=blue helm/app | kubectl delete -f -
```