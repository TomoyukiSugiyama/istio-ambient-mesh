# apply deployment and service

```bash
helm template --set color=blue helm/app | kubectl apply -f -
helm template --set color=green helm/app | kubectl apply -f -
```

# apply gateway and httpRoute

```bash
helm template helm/gateway | kubectl apply -f -
helm template helm/route | kubectl apply -f -
```

# apply curl app

```bash
helm template  --set name=accept-test helm/test | kubectl apply -f -
helm template  --set name=deny-test helm/test | kubectl apply -f -
```

## access test before apply L4 AuthorizationPolicy

accept-test-deployment
```bash
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-service-blue.default.svc.cluster.local
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-service-green.default.svc.cluster.local
# access from gateway
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:blue"
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:green"
```

deny-test-deployment
```bash
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-service-blue.default.svc.cluster.local
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-service-green.default.svc.cluster.local
# access from gateway
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:blue"
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:green"
```

# apply L4 AuthorizationPolicy

accept from:

* cluster.local/ns/default/sa/accept-test-service-account
* cluster.local/ns/istio-system/sa/sample-gateway-istio


```
helm template helm/authorization-policy | kubectl apply -f -
```

# access test

## success

```
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-service-blue.default.svc.cluster.local
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-service-green.default.svc.cluster.local
# access from gateway
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:blue"
kubectl exec deployments/accept-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:green"
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:blue"
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-gateway-istio.istio-system.svc.cluster.local -H "version:green"
```

## fail

```bash
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-service-blue.default.svc.cluster.local
kubectl exec deployments/deny-test-deployment -- curl -s http://sample-service-green.default.svc.cluster.local
```

# clean

```bash
helm template helm/authorization-policy | kubectl delete -f -
helm template  --set name=deny-test helm/test | kubectl delete -f -
helm template  --set name=accept-test helm/test | kubectl delete -f -
helm template helm/route | kubectl delete -f -
helm template helm/gateway | kubectl delete -f -
helm template --set color=green helm/app | kubectl delete -f -
helm template --set color=blue helm/app | kubectl delete -f -
```