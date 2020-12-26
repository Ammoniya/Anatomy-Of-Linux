<h2> Install kubernetes </h2> <img src="https://user-images.githubusercontent.com/20130001/86042532-ed5ccf80-ba64-11ea-9e0e-2b844cbc0d00.png" alt="drawing" width="200"/>

## Installation
#### Disable swap
```
sudo swapoff -a
```

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
```
#### Set kubectl to executable mode without sudo and point kubectl to the Kubernetes API server 
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
mkdir -p $HOME/.kube
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
#### Install pod network
```
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
gcloud projects add-iam-policy-binding <PROJECT>
                                --member "serviceAccount:SERVICE_ACCOUNT_NAME@PROJECT_ID.iam.gserviceaccount.com"
                                --role "roles/ROLE"
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
#### Kubectl Context and Configuration 
```
kubectl config view # Show Merged kubeconfig settings.

# use multiple kubeconfig files at the same time and view merged config
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2 

kubectl config view

# get the password for the e2e user
kubectl config view -o jsonpath='{.users[?(@.name == "e2e")].user.password}'

kubectl config view -o jsonpath='{.users[].name}'    # display the first user
kubectl config view -o jsonpath='{.users[*].name}'   # get a list of users
kubectl config get-contexts                          # display list of contexts 
kubectl config current-context                       # display the current-context
kubectl config use-context my-cluster-name           # set the default context to my-cluster-name

# add a new user to your kubeconf that supports basic auth
kubectl config set-credentials kubeuser/foo.kubernetes.com --username=kubeuser --password=kubepassword

# permanently save the namespace for all subsequent kubectl commands in that context.
kubectl config set-context --current --namespace=ggckad-s2

# set a context utilizing a specific username and namespace.
kubectl config set-context gce --user=cluster-admin --namespace=foo \
  && kubectl config use-context gce
 
kubectl config unset users.foo                       # delete user foo
```
#### Apply
```
Kubernetes manifests can be defined in YAML or JSON. The file extension .yaml, .yml, and .json can be used.

kubectl apply -f ./my-manifest.yaml            # create resource(s)
kubectl apply -f ./my1.yaml -f ./my2.yaml      # create from multiple files
kubectl apply -f ./dir                         # create resource(s) in all manifest files in dir
kubectl apply -f https://git.io/vPieo          # create resource(s) from url
kubectl create deployment nginx --image=nginx  # start a single instance of nginx
kubectl explain pods                           # get the documentation for pod manifests
```
#### Viewing, Finding Resources
```
# Get commands with basic output
kubectl get services                          # List all services in the namespace
kubectl get pods --all-namespaces             # List all pods in all namespaces
kubectl get pods -o wide                      # List all pods in the current namespace, with more details
kubectl get deployment my-dep                 # List a particular deployment
kubectl get pods                              # List all pods in the namespace
kubectl get pod my-pod -o yaml                # Get a pod's YAML

# Describe commands with verbose output
kubectl describe nodes my-node
kubectl describe pods my-pod

# List Services Sorted by Name
kubectl get services --sort-by=.metadata.name

# List pods Sorted by Restart Count
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'

# List PersistentVolumes sorted by capacity
kubectl get pv --sort-by=.spec.capacity.storage

# Get the version label of all pods with label app=cassandra
kubectl get pods --selector=app=cassandra -o \
  jsonpath='{.items[*].metadata.labels.version}'

# Retrieve the value of a key with dots, e.g. 'ca.crt'
kubectl get configmap myconfig \
  -o jsonpath='{.data.ca\.crt}'

# Get all worker nodes (use a selector to exclude results that have a label
# named 'node-role.kubernetes.io/master')
kubectl get node --selector='!node-role.kubernetes.io/master'

# Get all running pods in the namespace
kubectl get pods --field-selector=status.phase=Running

# Get ExternalIPs of all nodes
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'

# List Names of Pods that belong to Particular RC
# "jq" command useful for transformations that are too complex for jsonpath, 
it can be found at https://stedolan.github.io/jq/
sel=${$(kubectl get rc my-rc --output=json | jq -j '.spec.selector | to_entries | .[] | "\(.key)=\(.value),"')%?}
echo $(kubectl get pods --selector=$sel --output=jsonpath={.items..metadata.name})

# Show labels for all pods (or any other Kubernetes object that supports labelling)
kubectl get pods --show-labels

# List Events sorted by timestamp
kubectl get events --sort-by=.metadata.creationTimestamp

# Compares the current state of the cluster against the state that the cluster would be in if the manifest was applied.
kubectl diff -f ./my-manifest.yaml
```
#### Updating Resources
```
# Rolling update "www" containers of "frontend" deployment, updating the image
kubectl set image deployment/frontend www=image:v2

# Check the history of deployments including the revision 
kubectl rollout history deployment/frontend

# Rollback to the previous deployment
kubectl rollout undo deployment/frontend

# Rollback to a specific revision
kubectl rollout undo deployment/frontend --to-revision=2

# Watch rolling update status of "frontend" deployment until completion
kubectl rollout status -w deployment/frontend

# Rolling restart of the "frontend" deployment
kubectl rollout restart deployment/frontend            

# Replace a pod based on the JSON passed into std
cat pod.json | kubectl replace -f -                     

# Force replace, delete and then re-create the resource. Will cause a service outage.
kubectl replace --force -f ./pod.json

# Create a service for a replicated nginx, which serves on port 80 and connects to the containers on port 8000
kubectl expose rc nginx --port=80 --target-port=8000

# Update a single-container pod's image version (tag) to v4
kubectl get pod mypod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -

kubectl label pods my-pod new-label=awesome                      # Add a Label
kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq       # Add an annotation
kubectl autoscale deployment foo --min=2 --max=10                # Auto scale a deployment "foo"
```
#### Patching Resources 
```
# Partially update a node
kubectl patch node k8s-node-1 -p '{"spec":{"unschedulable":true}}'

# Update a container's image; spec.containers[*].name is required because it's a merge key
kubectl patch pod valid-pod -p '
{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'

# Update a container's image using a json patch with positional arrays
kubectl patch pod valid-pod --type='json' -p=
'[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'

# Disable a deployment livenessProbe using a json patch with positional arrays
kubectl patch deployment valid-deployment  --type json   -p=
'[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

# Add a new element to a positional array
kubectl patch sa default --type='json' -p=
'[{"op": "add", "path": "/secrets/1", "value": {"name": "whatever" } }]'
```
#### Editing Resources
```
kubectl edit svc/docker-registry                      # Edit the service named docker-registry
KUBE_EDITOR="nano" kubectl edit svc/docker-registry   # Use an alternative editor
```
#### Scaling Resources
```
 # Scale a replicaset named 'foo' to 3
kubectl scale --replicas=3 rs/foo

# Scale a resource specified in "foo.yaml" to 3
kubectl scale --replicas=3 -f foo.yaml

# If the deployment named mysql's current size is 2, scale mysql to 3
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql

# Scale multiple replication controllers
kubectl scale --replicas=5 rc/foo rc/bar rc/baz                   
```
#### Deleting Resources
```
# Delete a pod using the type and name specified in pod.json
kubectl delete -f ./pod.json

# Delete pods and services with same names "baz" and "foo"
kubectl delete pod,service baz foo

# Delete pods and services with label name=myLabel
kubectl delete pods,services -l name=myLabel

# Delete all pods and services in namespace my-ns,
kubectl -n my-ns delete pod,svc --all

# Delete all pods matching the awk pattern1 or pattern2
kubectl get pods  -n mynamespace --no-headers=true | awk '/pattern1|pattern2/{print $1}' | xargs  kubectl delete -n mynamespace pod
```
#### Interacting with running Pods
```
 # dump pod logs (stdout)
kubectl logs my-pod

# dump pod logs, with label name=myLabel (stdout)
kubectl logs -l name=myLabel

# dump pod logs (stdout) for a previous instantiation of a container
kubectl logs my-pod --previous

# dump pod container logs (stdout, multi-container case)
kubectl logs my-pod -c my-container

# dump pod logs, with label name=myLabel (stdout)
kubectl logs -l name=myLabel -c my-container

# dump pod container logs (stdout, multi-container case) for a previous instantiation of a container
kubectl logs my-pod -c my-container --previous

# stream pod logs (stdout)
kubectl logs -f my-pod

# stream pod container logs (stdout, multi-container case)
kubectl logs -f my-pod -c my-container

# stream all pods logs with label name=myLabel (stdout)
kubectl logs -f -l name=myLabel --all-containers

# Run pod as interactive shell
kubectl run -i --tty busybox --image=busybox -- sh

# Run pod nginx in a specific namespace
kubectl run nginx --image=nginx -n mynamespace

# Run pod nginx and write its spec into a file called pod.yaml
kubectl run nginx --image=nginx                     
--dry-run=client -o yaml > pod.yaml

# Attach to Running Container
kubectl attach my-pod -i

# Listen on port 5000 on the local machine and forward to port 6000 on my-pod
kubectl port-forward my-pod 5000:6000

# Run command in existing pod (1 container case)
kubectl exec my-pod -- ls /

# Run command in existing pod (multi-container case)
kubectl exec my-pod -c my-container -- ls /

 # Show metrics for a given pod and its containers
kubectl top pod POD_NAME --containers              
```
#### Interacting with Nodes and Cluster 
```
# Mark my-node as unschedulable
kubectl cordon my-node

# Drain my-node in preparation for maintenance
kubectl drain my-node

# Mark my-node as schedulable
kubectl uncordon my-node

# Show metrics for a given node
kubectl top node my-node

# Display addresses of the master and services
kubectl cluster-info

# Dump current cluster state to stdout
kubectl cluster-info dump

# Dump current cluster state to /path/to/cluster-state
kubectl cluster-info dump --output-directory=/path/to/cluster-state   

# If a taint with that key and effect already exists, its value is replaced as specified.
kubectl taint nodes foo dedicated=special-user:NoSchedule
```
#### Resource types 
```
kubectl api-resources


# All namespaced resources
kubectl api-resources --namespaced=true

# All non-namespaced resources
kubectl api-resources --namespaced=false

 # All resources with simple output (just the resource name)
kubectl api-resources -o name

# All resources with expanded (aka "wide") output
kubectl api-resources -o wide

# All resources that support the "list" and "get" request verbs
kubectl api-resources --verbs=list,get

# All resources in the "extensions" API group
kubectl api-resources --api-group=extensions 
```



#### Check Performance
```
| Name                                         | Command                                            |
|----------------------------------------------+----------------------------------------------------|
| Get node resource usage                      | kubectl top node                                   |
| Get pod resource usage                       | kubectl top pod                                    |
| Get resource usage for a given pod           | kubectl top <podname> --containers                 |
| List resource utilization for all containers | kubectl top pod --all-namespaces --containers=true |
```
#### Resources Deletion
```
| Name                                    | Command                                                 |
|-----------------------------------------+---------------------------------------------------------|
| Delete pod                              | kubectl delete pod/<pod-name> -n <my-namespace>         |
| Delete pod by force                     | kubectl delete pod/<pod-name> --grace-period=0 --force  |
| Delete pods by labels                   | kubectl delete pod -l env=test                          |
| Delete deployments by labels            | kubectl delete deployment -l app=wordpress              |
| Delete all resources filtered by labels | kubectl delete pods,services -l name=myLabel            |
| Delete resources under a namespace      | kubectl -n my-ns delete po,svc --all                    |
| Delete persist volumes by labels        | kubectl delete pvc -l app=wordpress                     |
| Delete state fulset only (not pods)     | kubectl delete sts/<stateful_set_name> --cascade=false  |
```

#### Log & Conf Files
```
| Name                      | Comment                                                                   |
|---------------------------+---------------------------------------------------------------------------|
| Config folder             | /etc/kubernetes/                                                          |
| Certificate files         | /etc/kubernetes/pki/                                                      |
| Credentials to API server | /etc/kubernetes/kubelet.conf                                              |
| Superuser credentials     | /etc/kubernetes/admin.conf                                                |
| kubectl config file       | ~/.kube/config                                                            |
| Kubernets working dir     | /var/lib/kubelet/                                                         |
| Docker working dir        | /var/lib/docker/,/var/log/containers/                                     |
| Etcd working dir          | /var/lib/etcd/                                                            |
| Network cni               | /etc/cni/net.d/                                                           |
| Log files                 | /var/log/pods/                                                            |
| log in worker node        | /var/log/kubelet.log,/var/log/kube-proxy.log                              |
| log in master node        | kube-apiserver.log,kube-scheduler.log,kube-controller-manager.log         |
| Env                       | /etc/systemd/system/kubelet.service.d/10-kubeadm.conf                     |
| Env                       | export KUBECONFIG=/etc/kubernetes/admin.conf                              |
```
#### Pod
```

| Name                         | Command                                                                                   |
|------------------------------+-------------------------------------------------------------------------------------------|
| List all pods                | kubectl get pods=                                                                         |
| List pods for all namespace  | kubectl get pods -all-namespaces=                                                         |
| List all critical pods       | kubectl get -n kube-system pods -a=                                                       |
| List pods with more info     | kubectl get pod -o wide=, =kubectl get pod/<pod-name> -o yaml=                            |
| Get pod info                 | kubectl describe pod/srv-mysql-server=                                                    |
| List all pods with labels    | kubectl get pods --show-labels=                                                           |
| List running pods            | kubectl get pods --field-selector=status.phase=Running                                    |
| Get Pod initContainer status | kubectl get pod --template '{{.status.initContainerStatuses}}' <pod-name>=                |
| kubectl run command          | kubectl exec -it -n "$ns" "$podname" -- sh -c "echo $msg >>/dev/err.log"                  |
| Watch pods                   | kubectl get pods  -n wordpress --watch=                                                   |
| Get pod by selector          | kubectl get pods --selector="app=syslog" -o jsonpath='{.items[*].metadata.name}'          |
| List pods and images         | kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image' |
| List pods and containers     | -o='custom-columns=PODS:.metadata.name,CONTAINERS:.spec.containers[*].name'               |
```
#### Label & Annontation
```
| Name                             | Command                                                           |
|----------------------------------+-------------------------------------------------------------------|
| Filter pods by label             | kubectl get pods -l owner=denny                                   |
| Manually add label to a pod      | kubectl label pods dummy-input owner=denny                        |
| Remove label                     | kubectl label pods dummy-input owner-                             |
| Manually add annonation to a pod | kubectl annotate pods dummy-input my-url=https://dennyzhang.com   |
```

#### Deployment & Scale
```

| Name                         | Command                                                                  |
|------------------------------+--------------------------------------------------------------------------|
| Scale out                    | kubectl scale --replicas=3 deployment/nginx-app                          |
| online rolling upgrade       | kubectl rollout app-v1 app-v2 --image=img:v2                             |
| Roll backup                  | kubectl rollout app-v1 app-v2 --rollback                                 |
| Check update status          | kubectl rollout status deployment/nginx-app                              |
| Check update history         | kubectl rollout history deployment/nginx-app                             |
| Pause/Resume                 | kubectl rollout pause deployment/nginx-deployment, resume                |
| Rollback to previous version | kubectl rollout undo deployment/nginx-deployment                         |
| List rollout                 | kubectl get rs                                                           |
```

#### Quota & Limits & Resource

```
| Name                          | Command                                                                 |
|-------------------------------+-------------------------------------------------------------------------|
| List Resource Quota           | kubectl get resourcequota                                               |
| List Limit Range              | kubectl get limitrange                                                  |
| Customize resource definition | kubectl set resources deployment nginx -c=nginx --limits=cpu=200m       |
| Customize resource definition | kubectl set resources deployment nginx -c=nginx --limits=memory=512Mi   |
```

#### Service


```
| Name                            | Command                                                                           |
|---------------------------------+-----------------------------------------------------------------------------------|
| List all services               | kubectl get services                                                              |
| List service endpoints          | kubectl get endpoints                                                             |
| Get service detail              | kubectl get service nginx-service -o yaml                                         |
| Get service cluster ip          | kubectl get service nginx-service -o go-template='{{.spec.clusterIP}}'            |
| Get service cluster port        | kubectl get service nginx-service -o go-template='{{(index .spec.ports 0).port}}' |
| Expose deployment as lb service | kubectl expose deployment/my-app --type=LoadBalancer --name=my-service            |
| Expose service as lb service    | kubectl expose service/wordpress-1-svc --type=LoadBalancer --name=ns1             |
```

#### Secrets

```
| Name                             | Command                                                                  |
|----------------------------------+--------------------------------------------------------------------------|
| List secrets                     | kubectl get secrets --all-namespaces                                     |
| Generate secret                  | echo -n 'mypasswd', then redirect to base64 --decode                     |
| Get secret                       | kubectl get secret denny-cluster-kubeconfig                              |
| Get a specific field of a secret | kubectl get secret denny-cluster-kubeconfig -o jsonpath="{.data.value}"  |
| Create secret from cfg file      | kubectl create secret generic db-user-pass --from-file=./username.txt    |
```

#### StatefulSet

```
| Name                               | Command                                                  |
|------------------------------------+----------------------------------------------------------|
| List statefulset                   | kubectl get sts                                          |
| Delete statefulset only (not pods) | kubectl delete sts/<stateful_set_name> --cascade=false   |
| Scale statefulset                  | kubectl scale sts/<stateful_set_name> --replicas=5       |
```

#### Volumes & Volume Claims

```
| Name                      | Command                                                      |
|---------------------------+--------------------------------------------------------------|
| List storage class        |kubectl get storageclass                                      |
| Check the mounted volumes |kubectl exec storage ls /data                                 |
| Check persist volume      |kubectl describe pv/pv0001                                    |
| Copy local file to pod    |kubectl cp /tmp/my <some-namespace>/<some-pod>:/tmp/server    |
| Copy pod file to local    |kubectl cp <some-namespace>/<some-pod>:/tmp/server /tmp/my    |
```

#### Events & Metrics

```
| Name                            | Command                                                    |
|---------------------------------+------------------------------------------------------------|
| View all events                 | kubectl get events --all-namespaces                        |
| List Events sorted by timestamp | kubectl get events --sort-by=.metadata.creationTimestamp   |
```

#### Node Maintenance
```
| Name                                      | Command                       |
|-------------------------------------------+-------------------------------|
| Mark node as unschedulable                | kubectl cordon $NODE_NAME     |
| Mark node as schedulable                  | kubectl uncordon $NODE_NAME   |
| Drain node in preparation for maintenance | kubectl drain $NODE_NAME      |
```

#### Namespace & Security

```
| Name                          | Command                                                                                           |
|-------------------------------+---------------------------------------------------------------------------------------------------|
| List authenticated contexts   | kubectl config get-contexts=, =~/.kube/config                                                     |
| Set namespace preference      | kubectl config set-context <context_name> --namespace=<ns_name>                                   |
| Switch context                | kubectl config use-context <cluster-name>                                                         |
| Load context from config file | kubectl get cs --kubeconfig kube_config.yml                                                       |
| Delete the specified context  | kubectl config delete-context <cluster-name>                                                      |
| List all namespaces defined   | kubectl get namespaces                                                                            |
| List certificates             | kubectl get csr                                                                                   |
```

#### Network

```
| Name                              | Command                                                  |
|-----------------------------------+----------------------------------------------------------|
| Temporarily add a port-forwarding  | kubectl port-forward redis-134 6379:6379                |
| Add port-forwaring for deployment  | kubectl port-forward deployment/redis-master 6379:6379  |
| Add port-forwaring for replicaset  | kubectl port-forward rs/redis-master 6379:6379          |
| Add port-forwaring for service     | kubectl port-forward svc/redis-master 6379:6379         |
| Get network policy                 | kubectl get NetworkPolicy                               |
```

#### Patch

```
| Name                          | Summary                                                             |
|-------------------------------+---------------------------------------------------------------------|
| Patch service to loadbalancer |  kubectl patch svc $svc_name -p '{"spec": {"type": "LoadBalancer"}} |
```

#### Extenstions

```
| Name                                    | Summary                     |
|-----------------------------------------+-----------------------------|
| Enumerates the resource types available | kubectl api-resources       |
| List api group                          | kubectl api-versions        |
| List all CRD                            | kubectl get crd             |
| List storageclass                       | kubectl get storageclass    |
```

#### Common Commands

```
| Name                                 | Command                                                                                    |
|--------------------------------------+--------------------------------------------------------------------------------------------|
| Run curl test temporarily            | kubectl run --generator=run-pod/v1 --rm mytest --image=yauritux/busybox-curl -it           |
| Run wget test temporarily            | kubectl run --generator=run-pod/v1 --rm mytest --image=busybox -it wget                    |
| Run nginx deployment with 2 replicas | kubectl run my-nginx --image=nginx --replicas=2 --port=80                                  |
| Run nginx pod and expose it          | kubectl run my-nginx --restart=Never --image=nginx --port=80 --expose                      |
| Run nginx deployment and expose it   | kubectl run my-nginx --image=nginx --port=80 --expose                                      |
| List authenticated contexts          | kubectl config get-contexts=, =~/.kube/config                                              |
| Set namespace preference             | kubectl config set-context <context_name> --namespace=<ns_name>                            |
| List pods with nodes info            | kubectl get pod -o wide                                                                    |
| List everything                      | kubectl get all --all-namespaces                                                           |
| Get all services                     | kubectl get service --all-namespaces                                                       |
| Get all deployments                  | kubectl get deployments --all-namespaces                                                   |
| Show nodes with labels               | kubectl get nodes --show-labels                                                            |
| Get resources with json output       | kubectl get pods --all-namespaces -o json                                                  |
| Validate yaml file with dry run      | kubectl create --dry-run --validate -f pod-dummy.yaml                                      |
| Start a temporary pod for testing    | kubectl run --rm -i -t --image=alpine test-$RANDOM -- sh                                   |
| kubectl run shell command            | kubectl exec -it mytest -- ls -l /etc/hosts                                                |
| Get system conf via configmap        | kubectl -n kube-system get cm kubeadm-config -o yaml                                       |
| Get deployment yaml                  | kubectl -n denny-websites get deployment mysql -o yaml                                     |
| Explain resource                     | kubectl explain pods=, =kubectl explain svc                                                |
| Watch pods                           | kubectl get pods  -n wordpress --watch                                                     |
| Query healthcheck endpoint           | curl -L http://127.0.0.1:10250/healthz                                                     |
| Open a bash terminal in a pod        | kubectl exec -it storage sh                                                                |
| Check pod environment variables      | kubectl exec redis-master-ft9ex env                                                        |
| Enable kubectl shell autocompletion  | echo "source <(kubectl completion bash)" >>~/.bashrc=, and reload                          |
| Use minikube dockerd in your laptop  | eval $(minikube docker-env)=, No need to push docker hub any more                          |
| Kubectl apply a folder of yaml files | kubectl apply -R -f .                                                                      |
| Get services sorted by name          | kubectl get services --sort-by=.metadata.name                                              |
| Get pods sorted by restart count     | kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'                     |
| List pods and images                 | kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image'  |
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
