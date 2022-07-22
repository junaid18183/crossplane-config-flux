# crossplane-config-flux

Crossplane configuration that provides Composite Resource Claim `Flux` to deploy the fluxCD in your kubernetes cluster.

Under the hood it uses the [Crossplane Helm Provider](https://github.com/crossplane-contrib/provider-helm.git) and [flux community helm chart](https://github.com/fluxcd-community/helm-charts/tree/main/charts/flux2) to deploy the flux


# Getting Started

## Prerequsite 
1. Control plane Kubernetes cluster it can be a local kind cluster 
1. [Install the CrossPlane in your cluster](#install-the-crossplane-in-your-cluster)
1. Optional - Install the Crossplane CLI using the instructions [here](https://crossplane.io/docs/v1.9/getting-started/install-configure.html)


1. Create a target Kubernetes cluster

1. Create the secret in your local cluster in namespace crossplane-system with the kubeconfig of your target cluster where you want to install the flux.
      ```
      kubectl -n crossplane-system create secret generic cluster-config --from-file=kubeconfig=/tmp/vcluster-flux.yaml
      ```

1. Install this configuration. Use the [packages](https://github.com/junaid18183?tab=packages&repo_name=crossplane-config-flux) page to find the latest version.

    ```shell
    kubectl crossplane install configuration ghcr.io/junaid18183/crossplane-config-flux:<version>
    ```


1. Provision the FluxCD using the [Claims](#claim-flux) that are offered by this configuration.

----------------------

# Install the CrossPlane in your cluster

You can install the Crossplane in your local/control cluster using official helm chart

```bash
helm repo add crossplane-stable \
    https://charts.crossplane.io/stable

helm repo update

helm upgrade --install \
    crossplane crossplane-stable/crossplane \
    --namespace crossplane-system \
    --create-namespace \
    --wait
```

----------------------

# Claim: Flux

Use this Composite Resource Claim to provision a FluxCD in your cluster,

## Usage

1. Deploy FluxCD using `kubectl apply -f <filename>`. Here's an example [configuration:](examples/flux.yaml)

```yaml
apiVersion: ijuned.com/v1beta1
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

----------------------