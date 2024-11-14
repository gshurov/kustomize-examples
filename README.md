# Kustomize examples

## Introduction

**Kustomize** is a simple and easy-to-use tool that ‘lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is’ (from [the official Kustomize repository](https://github.com/kubernetes-sigs/kustomize/)).

The main tool for customising existing application manifests, are similar _Kustomize_ manifests, `kustomization.yaml`, which describe in a declarative way a set of resources (common k8s resource manifests such as Deployments, Services, Config Maps, etc.) and a set of changes applied to them (generators, patches, etc.). 

When using _Kustomize_, it is common to follow a certain hierarchy of files: a `base` folder with the most generic versions of application manifests and an `overlays` folder where layers or specific environments for which manifests will be configured (e.g. dev, staging, prod, etc.) are located.

Despite being a separate product, as of _kubectl_ version 1.14, _Kustomize is integrated into kubectl_. (The version can be viewed with `kubectl version`). So Kustomize manifests can be applied using `kubectl apply -k path\to\foldet\with\kustomization\file`. And to look at the generated manifests in advance without applying them, just type `kubectl kustomize`.

## Overview
There are 3 examples of Kustomize usage (each of them has its own README):
- Comparison of manifest structure with and without Kustomize in the `with-and-without-kustomize` folder;
- Comparison of Kustomize projects with and without Kustomize components in the `kustomize-components` folder;
- Example of using Kustomize with Helm in the `helm-with-kustomize` folder.