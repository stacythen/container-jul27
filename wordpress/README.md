# Workshop

## Deploy DB

kubectl apply -f 01-wpdb-pwc.yaml -n wp-ns
kubectl apply -f 02-wpdb.yaml -n wp-ns

## Deploy App

kubectl apply -f 03-wpcontent-pwc.yaml -n wp-ns
kubectl apply -f 04-wpapp.yaml -n wp-ns

#Verify DB

kubectl exec -ti pod/wpdb-deploy-599776d488-vmsrh -- /bash/ssh
