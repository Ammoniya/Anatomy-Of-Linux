<h1><b>Setup instructions</b></h1>

<h2> Ubuntu configurarions </h2><img src="https://user-images.githubusercontent.com/20130001/86043076-c94dbe00-ba65-11ea-8794-2b8c8a922b34.png" alt="drawing" width="200" height="150"/> 

#### Verify the MAC address and product_uuid are unique for every node
```
ip link
sudo cat /sys/class/dmi/id/product_uuid

//if not bridge iptables

sudo sysctl --system
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```
#### If ubuntu firewall(ufw) is enabled 
```
sudo ufw status
sudo ufw status verbose

//if ufw enables 

sudo ufw allow 6443/tcp
sudo ufw allow 2379/tcp
sudo ufw allow 2380/tcp
sudo ufw allow 10250/tcp
sudo ufw allow 10251/tcp
sudo ufw allow 10252/tcp
sudo ufw allow 10255/tcp
sudo ufw allow 10251/tcp
sudo ufw allow 10255/tcp
sudo ufw reload
```
#### disable swap
```
sudo swapoff -a
```
<h2> Install docker-ce </h2> <img src="https://user-images.githubusercontent.com/20130001/86041084-c2717c00-ba62-11ea-9437-120d650c88d4.png " alt="drawing" width="200"/>

#### Note do not install docker.io it's old

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
```
#### Check docker is up and running 
```
docker -v
sudo systemctl status docker 

//if it's not up and running 
sudo systemctl enable docker
sudo systemctl start docker
```
#### Add docker to sudo [optional]
```
sudo usermod -aG docker sudo
```
<h2> Install kubernetes </h2> <img src="https://user-images.githubusercontent.com/20130001/86042532-ed5ccf80-ba64-11ea-9e0e-2b844cbc0d00.png" alt="drawing" width="200"/>

#### Install kubernetes [kubelet kubeadm kubectl]
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

MASTER - 
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl 

WORKER-
sudo apt-get install -y kubelet kubeadm
sudo apt-mark hold kubelet kubeadm
```
#### Check Cgroup [docker-ce and Kubernetes must be in the same Cgraoup]
```
sudo docker info | grep -i cgroup
cat /var/lib/kubelet/config.yaml
```
#### If not make Kubernetes to docker Cgroup [In docker-ce this is not mandatory , k8 and docker work even they are in systemd and  cgroupfs]
```
vim var/lib/kubelet/config.yaml

edit ~
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: cgroupfs
~

systemctl daemon-reload
systemctl restart kubelet
```
#### Start kubernetes master 
```
sudo kubeadm init --pod-network-cidr=*.*.*.* [Ex- 10.244.0.0/16]

NOTE - If kubeadm is used and you are planning to use coreos flannel  as your network,
 then pass --pod-network-cidr=10.244.0.0/16
 to kubeadm init to ensure that the podCIDR is set

MAKE SURE TO - copy the join command that gives token and certificate to add workers 

```
#### Set kubectl to executable mode without sudo and point kubectl to the Kubernetes API server 
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
mkdir -p $HOME/.kube
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
#### Install pod network
```
The flannel manifest defines four things:

1. A ClusterRole and ClusterRoleBinding for role-based access control (RBAC).
2. A service accounts for flannel to use.
3. A ConfigMap containing both a CNI configuration and a flannel configuration. 
The network in the flannel configuration should match the pod network CIDR.
 The choice of backend is also made here and defaults to VXLAN.
4. A DaemonSet to deploy the flannel pod on each Node.
 The pod has two containers
       1) the flannel daemon itself, and 
       2) an initContainer for deploying the CNI configuration to a location that the kubelet can read.


INSTALL: 
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
#### Check pod network is up and running
```
kubectl get pods --all-namespaces
kubectl get nodes // At the moment you will see only master
```
#### Add wokers
```
#set verbose level to six [--v=6] if you want to see log
kubeadm join --discovery-token ************** --discovery-token-ca-cert-hash *************** 
```
#### Check workers up and running with the master
```
kubectl get nodes
kubectl get nodes -o wide // view IP and all
```
#### Set up the node role
```
kubectl label node "node-host-name" node-role.kubernetes.io/worker=worker [node-role.kubernetes.io/'key=value']
```
#### Get new token and certificate to add new node later, after token expired
```
kubeadm token create --print-join-command
```
#### Dynamic provisioning with NFS
##### Create clusterrole, clusterrolebinding, role and rolebinding
```
kind: ServiceAccount
apiVersion: v1
metadata:
  name: nfs-client-provisioner
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: nfs-client-provisioner-runner
rules:
  - apiGroups: [""]
    resources: ["persistentvolumes"]
    verbs: ["get", "list", "watch", "create", "delete"]
  - apiGroups: [""]
    resources: ["persistentvolumeclaims"]
    verbs: ["get", "list", "watch", "update"]
  - apiGroups: ["storage.k8s.io"]
    resources: ["storageclasses"]
    verbs: ["get", "list", "watch"]
  - apiGroups: [""]
    resources: ["events"]
    verbs: ["create", "update", "patch"]
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: run-nfs-client-provisioner
subjects:
  - kind: ServiceAccount
    name: nfs-client-provisioner
    namespace: default
roleRef:
  kind: ClusterRole
  name: nfs-client-provisioner-runner
  apiGroup: rbac.authorization.k8s.io
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: leader-locking-nfs-client-provisioner
rules:
  - apiGroups: [""]
    resources: ["endpoints"]
    verbs: ["get", "list", "watch", "create", "update", "patch"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: leader-locking-nfs-client-provisioner
subjects:
  - kind: ServiceAccount
    name: nfs-client-provisioner
    # replace with namespace where provisioner is deployed
    namespace: default
roleRef:
  kind: Role
  name: leader-locking-nfs-client-provisioner
  apiGroup: rbac.authorization.k8s.io
```
save the file as rbac.yaml and run ``` kubectl create -f rbac.yaml ```
##### Create custom NFS provisioner 
```
kind: Deployment
apiVersion: apps/v1
metadata:
  name: nfs-client-provisioner
spec:
  replicas: 1
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: nfs-client-provisioner
  template:
    metadata:
      labels:
        app: nfs-client-provisioner
    spec:
      serviceAccountName: nfs-client-provisioner
      containers:
        - name: nfs-client-provisioner
          image: quay.io/external_storage/nfs-client-provisioner:latest
          volumeMounts:
            - name: nfs-client-root
              mountPath: /persistentvolumes
          env:
            - name: PROVISIONER_NAME
              value: cgraph.com/nfs
            - name: NFS_SERVER
              value: 10.4.48.42
            - name: NFS_PATH
              value: /cyber-cgraph
      volumes:
        - name: nfs-client-root
          nfs:
            server: 10.4.48.42
            path: /cyber-cgraph
```
save the file as deployment.yaml and run ``` kubectl create -f deployment.yaml ```
##### Create storage class
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: managed-nfs-storage
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: cgraph.com/nfs
parameters:
  archiveOnDelete: "false"
```
save the file as sclass.yaml and run ``` kubectl create -f sclass.yaml ```
#### Create global private container repository for kubernetes cluster with GCR
##### Configure GCP-GCR service 
initialize the project
```
gcloud init
```
config GCR as docker remote repository
```
 gcloud auth configure-docker
```
create service account, create IAM , bind service account with IAM for GCP and get private key to authenticate with GCP
```
gcloud iam service-accounts create SERVICE_ACCOUNT_NAME
gcloud projects add-iam-policy-binding cogent-sweep-274405 --member "serviceAccount:SERVICE_ACCOUNT_NAME@PROJECT_ID.iam.gserviceaccount.com" --role "roles/ROLE"
gcloud iam service-accounts keys create keyfile.json --iam-account SERVICE_ACCOUNT_NAME@PROJECT_ID.iam.gserviceaccount.com
```
##### Docker local remote container repository as GCR
ssh to each node of k8 cluster then
```
sudo nano /etc/docker/daemon.json
{
  "insecure-registries": ["gcr.io","docker.io"]
}
sudo systemctl restart docker
sudo systemctl docker status
```

<h2> Install helm on the master </h2><img src="https://user-images.githubusercontent.com/20130001/86042862-6eb46200-ba65-11ea-9702-fbcb351b1060.png" alt="drawing" width="200"/> 

#### Install helm on the master
```
Helm is a package manager for Kubernetes that allows developers and operators to more easily package,
configure, and deploy applications and services onto Kubernetes clusters.

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

or use snap/ubuntu  

sudo snap install helm --classic
```
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