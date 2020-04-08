# GKE Internal TCP Load Balancer with Nginx Ingress Controller

This is an example of setting up Nginx Ingress Controller on GKE through TCP Internal Load Balancer

## Install Nginx Ingress Controller

```sh
helm install nginx-internal-ingress stable/nginx-ingress -f internal-ingress.yaml
```

## Inspect Nginx Controller

Get Nginx Controller LB private IP

```sh
kubectl get svc nginx-internal-ingress-nginx-ingress-controller -ojson | jq -r '.status.loadBalancer.ingress[].ip'
```

## Deploy demo app

```sh
# k8s deployment
kubectl apply -f deploy.yaml
# K8s service
kubectl apply -f service.yaml
# ingress resource
kubectl apply -f ingress.yaml
```

## Test

```sh
# run alpine with sh
kubectl run -it --rm ingress-test --image alpine

# inside alpine container
wget -q -O - http://<INTERNAL_LB_IP>

# expected output - hostname will change
Hello, world!
Version: 2.0.0
Hostname: hello-app-5f9d7479bd-cn67d
```

## Cleanup

```sh
kubectl delete -f ingress.yaml
kubectl delete -f service.yaml
kubectl delete -f deploy.yaml
helm delete nginx-internal-ingress
```
