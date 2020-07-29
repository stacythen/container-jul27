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

## Rolling Update

```bash
kubectl apply -f nwapp-v2.yaml -n nw --record
kubectl rollout history deploy/nwapp-deploy -n nw
```

## Rollback

```bash
kubectl rollout undo deploy/nwapp-deploy -n nw --to-revision=1
```

## Troubleshooting

do describe command

Example (troubleshooting pod):

```bash
kubectl describe pod -n nw
```

## Watch in Windows

```bash
for /l %g in () do @(kubectl get all -n nw -o wide && timeout /t 2)
```
