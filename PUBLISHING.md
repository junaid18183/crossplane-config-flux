## Publishing the Package

Download the crossplane plugin if not done already,

```bash
curl -sL https://raw.githubusercontent.com/crossplane/crossplane/release-1.5/install.sh | sh
sudo mv kubectl-crossplane /usr/local/bin
kubectl crossplane --help
```

Build the configuration package 

```
kubectl crossplane build configuration -f package/

[root@tiber flux_crossplane_package]# ls -l package/crossplane-flux-*.xpkg
-rw-r--r--. 1 vagrant vagrant 5632 Dec 24 12:31 package/crossplane-flux-b16c5eb3079f.xpkg

kubectl crossplane push configuration -f package/crossplane-flux-b16c5eb3079f.xpkg  ghcr.io/junaid18183/crossplane-config-flux
```

You can downlaod the configuration now, 

```
kubectl crossplane install configuration ghcr.io/junaid18183/crossplane-config-flux
configuration.pkg.crossplane.io/junaid18183-crossplane-flux created

kubectl get configurations.pkg.crossplane.io
NAME                          INSTALLED   HEALTHY   PACKAGE                             AGE
junaid18183-crossplane-flux   True        True      ghcr.io/junaid18183/crossplane-config-flux   15s
```

This configuration should install the `crossplane-provider-terraform` automatic, as the dependency is added on it.