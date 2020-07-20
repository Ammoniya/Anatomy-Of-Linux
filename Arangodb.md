<h2> Install ArangoDB [Cluster] </h2><img src="https://user-images.githubusercontent.com/20130001/87248999-2f7d0c80-c47a-11ea-8556-08fdf0090bdb.png" alt="drawing" width="200"/> 

#### Install arango db operators using HELM
```
helm install  https://github.com/arangodb/kube-arangodb/releases/download/1.0.3/kube-arangodb-1.0.3.tgz  --generate-name
helm install  https://github.com/arangodb/kube-arangodb/releases/download/1.0.3/kube-arangodb-crd-1.0.3.tgz  --generate-name
```
#### Create arango db cluster using resource YAML
save this file as arangodb-cluster.yaml and run ```kubectl apply -f  arangodb-cluster.yaml ```
```
apiVersion: "database.arangodb.com/v1alpha"
kind: "ArangoDeployment"
metadata:
  name: "cgraph-arangodb-cluster"
spec:
  mode: Cluster
  environment: Production
  agents:
    count: 3
    args:
      - --log.level=debug
    resources:
      requests:
        storage: 100Gi
    storageClassName: managed-nfs-storage
  dbservers:
    count: 5
    resources:
      requests:
        storage: 300Gi
    storageClassName: managed-nfs-storage
  coordinators:
    count: 5
  image: "arangodb/arangodb:3.6.4"
```

