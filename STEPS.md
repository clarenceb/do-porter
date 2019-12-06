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

porter credentials generate
Generating new credential spring-music from bundle spring-music
==> 4 credentials required for bundle spring-music
? How would you like to set credential "do_access_token" environment variable
? Enter the environment variable that will be used to set credential "do_access_token" DO_ACCESS_KEY
? How would you like to set credential "do_spaces_key" environment variable
? Enter the environment variable that will be used to set credential "do_spaces_key" DO_SPACES_KEY
? How would you like to set credential "do_spaces_secret" environment variable
? Enter the environment variable that will be used to set credential "do_spaces_secret" DO_SPACES_SECRET
? How would you like to set credential "kubeconfig" file path
? Enter the path that will be used to set credential "kubeconfig" <you-path-to>/kubeconfig
Saving credential to /home/<your-account>/.porter/credentials/spring-music.yaml
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
