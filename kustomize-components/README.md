# Use of Kustomize Components

This example will demonstrate the use of Kustomize components to add modularity to an application's deployment, as well as avoiding repetitive manifests.

The task is as follows: There is an application with three different environments/versions, **community**, **entreprise** and **dev** versions. Each version has features that overlap with the other versions: the community version must have support for an external database and reCAPTCHA to protect against bots; the enterprise version must have support for an external database and LDAP; the dev version must have all of the above with the ability to disable certain functionality. This can be visualised in the form of the following table:

|            | External DB  | reCAPTCHA     | LDAP          |
|------------|:------------:|:-------------:|:-------------:|
| Community  | Mandatorily  | Mandatorily   |               |
| Enterprise | Mandatorily  |               | Mandatorily   |
| Dev        | Optional     | Optional      | Optional      |

For the sake of clarity and simplicity, only 2 main manifests are used: the deployment and the config map for it.
In the non-componentised version, the structure is as follows (`without-components` folder):
- `base` folder, with a basic set of manifests;
- `overlays` folder, with a set of configuration options;
    - `community` folder, with manifests for community version;
    - `enterprise` folder, with manifests for the enterprise version;
    - `dev` folder, with manifests for the dev version.

It's not hard to see that configuration variants very often copy each other, using the same config map files, changing the deployment, etc. And this will certainly cause problems when we need to change, for example, the configuration of the external database, because changes will need to be made in several places, although we would like to avoid this when using Kustomize. To solve the problem of modularity in Kustomize we can use **components**.

But before that I would like to mention the **2nd option of using patches**, like in file `without-components\overlays\dev\deployment-patch.yaml`, where you can specify operations on adding, replacing or deleting list items, which can be very useful.

Components are another resource of Kustomize, in which you can also specify various generators and patches and which can be combined with other components. So when using components, the structure of the manifests changes (`with-components` folder):
- `base` folder, with the base set of manifests;
- `components` folder, with the configurations of all individual components;
    - `external_db` folder, with manifests for configuring the external database;
    - `ldap` folder, with manifests for configuring LDAP;
    - `recaptcha` folder, with manifests for configuring reCAPTCHA;
- `overlays` folder, with a set of configuration options;
    - `community` folder, with manifests for community version;
    - `enterprise` folder, with manifests for the enterprise version;
    - `dev` folder, with manifests for the dev version.

So manifests for configuration of all functional elements are allocated each in its own component, and in different versions there are just links to these components, for example in community version thera are links to the database and reCAPTCHA components, and in dev the LDAP is added to them.

Create namespaces to run this examples:
```
kubectl create ns community
kubectl create ns enterprise
kubectl create ns dev
```
Run examples with Kustomize with the command:
```
kubectl apply -k path\to\folder\with\kustomization\file
```
Use this command to see which manifest Kustomize generates from kustomization.yaml:
```
kubectl kustomize path\to\folder\with\kustomization\file
```