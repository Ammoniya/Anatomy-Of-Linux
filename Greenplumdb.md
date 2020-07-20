<h2> Install Greenplum DB [Cluster] </h2><img src="https://user-images.githubusercontent.com/20130001/87853080-d2270680-c924-11ea-822b-0d73fa16c9b6.png" alt="drawing" width="200"/>

```
tar xzf greenplum-for-kubernetes-*.tar.gz
cd ./greenplum-for-kubernetes-*
```

Load the Greenplum for Kubernetes Docker image to the local Docker registry:
```
sudo docker load -i ./images/greenplum-for-kubernetes
sudo docker load -i ./images/greenplum-operator
```

push the images to Google Cloud Registry using the current Google Cloud project name:

```
gcloud auth configure-docker

PROJECT=$(gcloud config list core/project --format='value(core.project)')
IMAGE_REPO="gcr.io/${PROJECT}"

GREENPLUM_IMAGE_NAME="${IMAGE_REPO}/greenplum-for-kubernetes:$(cat ./images/greenplum-for-kubernetes-tag)"
sudo docker tag $(cat ./images/greenplum-for-kubernetes-id) ${GREENPLUM_IMAGE_NAME}
sudo docker push ${GREENPLUM_IMAGE_NAME}

OPERATOR_IMAGE_NAME="${IMAGE_REPO}/greenplum-operator:$(cat ./images/greenplum-operator-tag)"
sudo docker tag $(cat ./images/greenplum-operator-id) ${OPERATOR_IMAGE_NAME}
sudo docker push ${OPERATOR_IMAGE_NAME}
```

Create a docker-registry secret regsecret for pods 
```
kubectl create secret  docker-registry  regsecret \
--docker-server=https://gcr.io \
--docker-username=_json_key \
--docker-password="$(cat key.json)"
```

Verify the regsecret.

```
./workspace/samples/scripts/regsecret-test.bash ${OPERATOR_IMAGE_NAME}
```
The output of above command should print ```GREENPLUM-OPERATOR TEST OK```

create operator-values-overrides manifest 

```
cat <<EOF >workspace/operator-values-overrides.yaml
operatorImageRepository: ${IMAGE_REPO}/greenplum-operator
greenplumImageRepository: ${IMAGE_REPO}/greenplum-for-kubernetes
EOF
```

create a new Greenplum Operator 

```
helm install greenplum-operator -f workspace/operator-values-overrides.yaml operator/
```

```
The command displays the following message and concludes with a link to this documentation:

NAME: greenplum-operator
LAST DEPLOYED: Wed May 13 09:55:06 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
greenplum-operator has been installed.

Please see documentation at:
http://greenplum-kubernetes.docs.pivotal.io/
```

view deployment stataus 

```
kubectl get serviceaccount

NAME                        SECRETS   AGE
default                     1         12m
greenplum-system-operator   1         8m56s

kubectl logs -l app=greenplum-operator
kubectl get pods --all-namespaces
```

status check
```
helm list
```

create resource file for cluster
```
apiVersion: "greenplum.pivotal.io/v1"
kind: "GreenplumCluster"
metadata:
  name: NAME
spec:
  masterAndStandby:
    hostBasedAuthentication: |
      # host   all   gpadmin   0.0.0.0/0    trust
    memory: "800Mi"
    cpu: "0.5"
    storageClassName: STORAGECLASS
    storage: 1G
    workerSelector: {}
  segments:
    primarySegmentCount: 1
    memory: "800Mi"
    cpu: "0.5"
    storageClassName: STORAGECLASS
    storage: 2G
    workerSelector: {}


kubectl apply -f ./cluster.yaml 
kubectl describe greenplumClusters/NAME
```
Delete the cluster
```
kubectl delete -f ./cluster.yaml  --wait=false
```
	