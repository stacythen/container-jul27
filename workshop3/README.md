# Workshop 3 - Advanced k8s

## Get Storage

```bash
kubectl get sc
```

## Annotations

is a key value pair, eg: for custom resources

Naming Convention:
`namespace/something`

fieldRef (metadata.annotations) is one of the example.

### Annotations vs Labels

`labels` is use for filtering while `annotations` is for system to consume

Sample Code:

```yaml
annotations:
  volume.beta.kubernetes.io/storage-provisioner: dobs.csi.digitalocean.com
```

# Getting Started

```bash
kubectl apply -f nwdb-pwc.yaml -n nw
```

## list all the resources supported in your k8s

Linux,

```bash
kubectl api-resources | less
```

## Cheatsheets

```bash
kubectl get persistentvolumeclaim -n nw
kubectl explain deploy
kubectl get all,pv,pvc -n nw
kubectl describe persistentvolumeclaim/nwdb-pvc -n nw
```

# Ingress

## Namespace

create namespace

```bash
kubectl create ns nginx-ingress
```

install `nginx-ingress`

```bash
helm install myingress stable/nginx-ingress --version 1.41.2 -f values.yaml -n nginx-ingress
```

Watch changes

```bash
watch kubectl get all -n nginx-ingress -o wide
```

```bash
helm ls -n nginx-ingress
```

## Deploy fortune

```bash
kubectl apply -f fortune.yaml -n fortune-ns
```

fake sub domain, the ip is the ingress external ip from Digital Ocean
[readme](https://nip.io/)
`fortune.139.59.223.108.nip.io`

Should see ingress resource created

```bash
kubectl get all,ing -n fortune-ns
```

## Manual way (incomplete)

manualpv.yaml

go to Digital Ocean, create volume

get volume name
doctl

## metric server

kubectl apply -f metrics-server.yaml

kubectl top nodes

kubectl describe pod/nwapp-deploy-6b8656d76d-xmw7x -n nw

## horizontal scale up

increase memory/cpu

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: nwapp-hpa
  labels:
    app: northwind
spec:
  minReplicas: 1
  maxReplicas: 4
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nwapp-deploy
  metrics:
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 15
```

```bash
kubectl get all,ing,hpa -n nw
```
