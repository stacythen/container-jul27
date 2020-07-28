# Getting Started

## create namespace `nw` using imperative way

```bash
kubectl create ns nw
```

## DB Deployment: nwdb-deploy

```bash
kubectl apply -f nwdb.yaml -n nw
```

## App Deployment: nwapp-deploy

```bash
kubectl apply -f nwapp.yaml -n nw
```

### Best Practice

Add `--record` so can rollback changes later

```bash
kubectl apply -f nwapp.yaml --record -n nw
```

Check history,

```bash
kubectl rollout history deploy/nwapp-deploy -n nw
```

## Scaling

```bash
kubectl scale deploy/nwapp-deploy --replicas=1 -n nw
```
