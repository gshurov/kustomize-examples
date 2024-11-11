# Kustomize with Helm

It is also possible to use Kustomize together with a templating tool such as Helm. This can be useful when it is not possible to modify the Chart repository on your own, but you need to make some changes.

In our case a small Helm Chart was taken, to which it was necessary to apply its values.yaml file and in which it was necessary to explicitly specify `nodePort` in the service, but Helm itself does not provide such an option.

The only difference compared to the `kustomization.yaml` files discussed in the other examples is the presence of a new `helmCharts` section where all the necessary information about the Chart is specified (name, repository, version, path to the values.yaml file, etc.). In the patch for the service, as before, it is necessary to specify the name and necessary changes, in our case explicitly specify the `nodePort` number.

It is worth specifying separately that using Helm in Kustomize, the Chart will be loaded into the file system in folder `chart`. In addition, to run the command on a build of manifests, **Helm must be installed** and the `--enable-helm` flag must be used, i.e. the command for a build will look like this:
```
kubectl kustomize --enable-helm path\to\folder\with\kustomization\file
```
Create namespace to run this example:
```
kubectl create ns dev
```
Run example with following command:
```
kubectl kustomize --enable-helm path\to\folder\with\kustomization\file | kubectl apply -f -
```
