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