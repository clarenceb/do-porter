Steps
=====

Pre-requisities:

* Create AKS cluster
* Create Digital Ocean account
* Create Digital Ocean API token and Spaces access key

```sh
source ./do-access.env
az aks get-credentials -g <resource-group> -n <cluster> -f ./kubeconfig
export KUBECONFIG=$(pwd)/kubeconfig
kubectl get nodes
```
