# Workshop 4

## install/debug presslabs/mysql

```bash
helm repo add presslabs https://presslabs.github.io/charts
helm repo update
kubectl create ns mysql-operator
helm install mysql-operator presslabs/mysql-operator --version 0.4.0 -n mysql-operator
kubectl describe pod/mysql-operator-0 -n mysql-operator
kubectl logs pod/mysql-operator-0 -n mysql-operator -c operator
kubectl logs pod/mysql-operator-0 -n mysql-operator -c orchestrator

kubectl get statefulset -n mysql-operator

```

## Create secret (Imperative way)

Example:

```bash
kubectl create secret generic mysecret --from-literal=KEY=value_in_plain_text
```

using Environment Variables

```bash
kubectl create secret generic mysecret --from-env-file=secret.env
kubectl get secret/mysecret -o yaml
```

## create nwdb-cluster

kubectl get svc -n nw -o wide

check selectors

## Migrate DB

Exec into the container kubectl exec -ti pod/nwdb-cluster-0 -c mysql -n nw -- /bin/sh
cd to /tmp
Login to mysql mysql -uroot -p
create the schema/database source ./0-northwind-schema.sql;
List the databases show databases;
Select northwind use northwind;
Load the data source ./1-northwind-data.sql;
Check the created tables; show tables;
Exit

Copy file into pod:

```bash
kubectl cp 0-northwind-schema.sql nwdb-cluster-mysql-0:/tmp -c mysql -n nw
kubectl exec -ti pod/nwdb-cluster-mysql-0 -c mysql -n nw -- /bin/bash
cd ./tmp
mysql -uroot -p
source ./0-northwind-schema.sql;
show databases;
use northwind;
source ./1-northwind-data.sql;
show tables;
```

## istio

istioctl install --set profile=demo

### manual injection

istioctl kube-inject -f bookinfo.yaml > bookinfo-sidecar.yaml
kubectl apply -f bookinfo-sidecar.yaml -n bookinfo

istioctl kube-inject -f bookinfo-ingress.yaml > bookinfo-ingress-inject.yaml

kubectl apply -f bookinfo-ingress-inject.yaml -n bookinfo
