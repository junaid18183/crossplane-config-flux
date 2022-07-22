## Publishing the Package

Download the crossplane plugin if not done already,

```bash
curl -sL https://raw.githubusercontent.com/crossplane/crossplane/release-1.5/install.sh | sh
sudo mv kubectl-crossplane /usr/local/bin
kubectl crossplane --help
```

Build the configuration package 

```
kubectl crossplane build configuration -f package/ --name flux-package.xpkg

[root@tiber flux_crossplane_package]# ls -l package/*.xpkg
-rw-r--r--  1 junedm  staff  5632 Jul 22 19:09 package/package.xpkg

kubectl crossplane push configuration -f package/flux-package.xpkg  ghcr.io/junaid18183/crossplane-config-flux:0.0.1
```

You can downlaod the configuration now, 

```
kubectl crossplane install configuration ghcr.io/junaid18183/crossplane-config-flux:0.0.1
configuration.pkg.crossplane.io/junaid18183-crossplane-flux created

kubectl get configurations.pkg.crossplane.io
NAME                          INSTALLED   HEALTHY   PACKAGE                             AGE
junaid18183-crossplane-flux   True        True      ghcr.io/junaid18183/crossplane-config-flux   15s
```

This configuration should install the `crossplane-provider-helm` automatic, as the dependency is added on it.