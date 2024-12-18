# Manifest structure with and without Kustomize

In this example you can see the main advantage of using Kustomize, namely avoiding copypasting. 
The task in the example is to write manifests for a small application with changes for different environments: for deployment and for production.

For this purpose, were written 3 manifests for the `app`: deployment, service and config map; and 3 manifests for the `database`: deployment, service and secret. A technical convention is that the Nginx image is used for both deployments, rather than the actual images, in order to simplify and reduce resource consumption. In the version where Kustomize is not used (in folder `without-kustomize`), the standard version of the development manifests are located in folder `without-kustomize\dev`, but it is needed to create also a production version, where it is necessary to change the number of replicas and resources for deployments, change secrets, configs and service types, and rename the namespace. This is done by just copying and manually changing all the required fields. The result can be found in the `without-kustomize\prod` folder. And everything would be fine as long as the project is small and only a couple of environments need to be configured, but problems arise when there are not 2 but, for example, 4 environments. This is where Kustomize comes to our rescue.

In the variant using Kustomize (folder `with-kustomize`) a base set of manifests is created (folder `with-kustomize\base`), which may contain some common settings, in our case it will be dev environment, but without manifest of secrets and configs. The `kustomization.yaml` manifest is mandatory, which lists all the resources used in the base set. Then an overlays folder (the `with-kustomize\overlays` folder) is created, which contains the config folders of the specific environments, in our case dev and prod. Now the environment folders will not contain the full set of manifests, but only the changes that need to be applied to the base configuration. In the case of the dev environment (`with-kustomize\overlays\dev` folder) it is just generating secrets and configs. Again you need to create a `kustomization.yaml` manifest listing all changes and a reference to the base configuration. In the case of creating a production environment, we need to make changes to our base set of manifests without changing them directly (folder `with-kustomize\overlays\prod`). This is achieved by using patches, files that contain configuration changes. Patches can be in 2 formats, (the second format is described in another example), but the format we use now is to specify the name, type, version of the api resource, and a list of changes to the specification, e.g. in the case of `app` it is the number of replicas and the resources occupied.

Create namespaces to run this examples:
```
kubectl create ns dev
kubectl create ns prod
```
Run examples with Kustomize with the command:
```
kubectl apply -k path\to\foldet\with\kustomization\file
```