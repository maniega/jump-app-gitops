# helm-demo

## Introduction

Helm Demo is one of a set of repositories developed to generate a microservice based application, named _Jump App_. This repository includes an automated way of deploy _Jump App's_ microservices and all staff around this application (Deployments, Services, Build Configs, Pipelines, etc). As probably known by the name of the repository, this automated "tool" is based on helm and tries to integrate the following solutions:

- Red Hat Openshift Container Platform 4 (\*Kubernetes)
- Multi programing language microservices (Javascript, Golang, Python and Java)
- GitOps solution, based on ArgoCD.
- CI/CD strategy, based Tekton.
- Service Mesh architecture, based on Istio.

This repository was created to include all automated procedures to achieve the following objectives:

- Create required namespaces in Kubernetes (jump-app-cicd, gitops, jump-app-dev, jump-app-pre and jump-app-pro)
- Install required ArgoCD objects (ArgoCD Server, Route, Rolebindings and Applications)
- Deploy CI/CD objects in jump-app-cicd namespace (Imagestreams, Tekton objects, etc)
- Deploy _Jump App's_ microservices in each environment/namespace
- Create Service Mesh objects

_NOTE:_ It is important to know that it is possible to activate/deactivate features through variable _enabled_ defined for each sub-chart in the global _values.yaml_ file.

## Requisites

In order to start working with this repository, it is required:

- A Red Hat Openshift Container Platform Cluster +4.5
- ArgoCD Operator installed
- Red Hat Openshift Pipelines Operator installed
- Helm client installed in the local machine (Please follow https://helm.sh/docs/intro/install/ for more information)

Optional requirements when Service Mesh is activated:

- Red Hat Openshift Service Mesh Operator installed
- Kiali Operator installed provided by Red Hat
- Red Hat Openshift Jager Operator installed
- Service Mesh Control Plane Object with default gateways (ingress and egress) configured
- Service Mesh Member Roll Object configured

## Quick Start

If the priority is making use of this solution and not waste any time, the following procedure install _Jump App_ and configure CI/CD and GitOps solutions automatically:

- Modify required variables files depends on environments

```$bash
git checkout [ feature/jump-app-cicd | feature/jump-app-pro | feature/jump-app-pre | feature/jump-app-dev ]
vi values.yml
git add -A
git commit -m "Modified apps domain"
ggpush
git checkout master
```

- Execute _setup.sh_ script

```$bash
oc login
cd scripts
./setup.sh
<answer questions>
```

**IMPORTANT**: By default, some namespaces will be created (_gitops-argocd_, _istio-system_, _jump-app-cicd_, _jump-app-dev_, _jump-app-pre_ and _jump-app-pro_). If it is required to modify their names, take special attention to modify associated variables and define the new names correctly.

## Access ArgoCD Console

Once ArgoCD Server is installed, it is possible access ArgoCD Web UI follow next procedure:

- Obtain admin password and ArgoCD Server URL

```$bash
$ oc login
$ oc get secret argocd-cluster -o jsonpath='{.data.admin\.password}' -n gitops-argocd | base64 -d
$ oc get route argocd -n gitops-argocd
argocd-gitops-argocd.apps.<mydomain>
```

- Review Application created object in respective namespaces (jump-app-cicd, jump-app-dev, jump-app-pre and jump-app-pro)

## Author Information

Asier Cidon

asier.cidon@gmail.com
