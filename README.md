# CrossPlane Configuration Package for the Flux Deployment

This Configuration Package depends on `crossplane/provider-terraform` provider. 

Under the hood it uses the [Terraform Provider Flux](https://github.com/fluxcd/terraform-provider-flux)  and [Terraform Provider kubectl](https://github.com/gavinbunney/terraform-provider-kubectl) 



### Install the CrossPlane Terraform Provider

Install the crossplane Terraform Provider in yout managent cluster

```bash
kubectl crossplane install provider crossplane/provider-terraform:v0.1.2

kubectl get providers
NAME                            INSTALLED   HEALTHY   PACKAGE                                AGE
crossplane-provider-terraform   True        True      crossplane/provider-terraform:v0.1.2   6h56m
```

Create the secret in crossplane-system with the `kubeconfig` of your target cluster where you want to install the flux. 

```bash
kubectl -n crossplane-system create secret generic cluster-config --from-file=kubeconfig=/tmp/vcluster-flux.yaml
```

### Deploy the Flux

You can deploy the flux using the CR as [below](./example-flux.yaml),

```yaml
apiVersion: juvensys.com/v1alpha1
kind: Flux
metadata:
  name: demo
spec:
  parameters:
    flux_version: v0.24.1
  kubeConfig:
    secretNamespace: crossplane-system
    secretName: cluster-config
    secretKey: kubeconfig
```

You can additioanly provide the following parameters 

| Name                         | Description                                                  | Default Value  |
| ---------------------------- | ------------------------------------------------------------ | -------------- |
| parameters/flux_version      | The the flux version to install                              | v0.24.1        |
| parameters/image_registry    | Container registry where the toolkit images are published    | ghcr.io/fluxcd |
| parameters/image_pull_secret | Kubernetes secret name used for pulling the toolkit images from a private registry | ''             |

```bash
kubectl apply -f example-flux.yaml
flux.juvensys.com/demo created
```



## Check status

You can check the status of the flux deployment using following kubectl

````bash
kubectl get flux,workspace
NAME                     READY   COMPOSITION      AGE
flux.juvensys.com/demo   False   flux-terraform   33s

NAME                              AGE
workspace.tf.crossplane.io/demo   32s
````

