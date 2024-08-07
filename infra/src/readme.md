# Manifests Deployment

Remember to always update the destination (cluster name) information in the yaml files.

### helm repo
```sh
helm repo add apache-airflow https://airflow.apache.org
helm repo add elastic https://helm.elastic.co
helm repo update
```

### minio [deepstorage]
```shell
kubectl apply -f app-manifests/deepstorage/minio-operator.yaml
kubectl apply -f app-manifests/deepstorage/minio-tenant.yaml
```

# Create the buckets manually 

```shell
kubens deepstorage
k get svc
```

Access datalake-console <External-ip>:port
  username: data-lake

  password: 12620ee6-2162-11ee-be56-0242ac120002

  - name: landing
  - name: metastore
  - name: trino

These steps will change the default storage class for your Minikube cluster. Now, any new PVCs created without specifying a storage class will use csi-hostpath-sc by default.

Here's how you can do it in a single command:
```shell
kubectl annotate storageclass standard storageclass.kubernetes.io/is-default-class- && kubectl annotate storageclass csi-hostpath-sc storageclass.kubernetes.io/is-default-class=true
```


### hive metastore [metastore]
```shell
kubectl apply -f app-manifests/metastore/hive-metastore.yaml
```

### trino [warehouse]
```shell
kubectl apply -f app-manifests/warehouse/trino.yaml
```

### airflow [orchestrator]
```shell
kubectl create secret generic airflow-fernet-key --from-literal=fernet-key='t5u8Dst5tkt1F5fwsxnfEwGfytY3Ry5KrP02B32mPxY=' --namespace orchestrator
kubectl apply -f git-credentials-secret.yaml --namespace orchestrator
kubectl apply -f app-manifests/orchestrator/airflow.yaml
```
