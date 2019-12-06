Steps
=====

### Pre-requisities

* Create AKS cluster
* Create Digital Ocean account
* Create Digital Ocean API token and Spaces access key

```sh
cp source ./do-access.env.template ./do-access.env
# Populate the Digital Ocean values in `do-access.env`
source ./do-access.env
az aks get-credentials -g <resource-group> -n <cluster> -f ./kubeconfig
export KUBECONFIG=$(pwd)/kubeconfig
kubectl get nodes
```

### Build the bundle

```sh
porter build
```

### Create the credentials

```sh
porter credentials generate
```

### Install the bundle

```sh
porter install -c spring-music --param space_name=do-porter --param database_name=do-porter-db
```

### Get the endpoint to view the app

```sh
porter show spring-music
# service_ip  string  xx.xx.xx.xx
```

Browse to: http://xx.xx.xx.xx

### Troubleshooting

Use the `--debug` flag with `porter`.

If there is a failure, run `porter upgrade` to retry.
The Digital Ocean Space need to be created first.  You can't re-run install or you'll get an error stating that the bucket already exists.
