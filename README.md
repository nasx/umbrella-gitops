# GitOps for Lab Clusters

[![GitHub Super-Linter](https://github.com/nasx/umbrella-gitops/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)

----

## About

This repository is used to configure the myriad OpenShift clusters I deploy in my lab using Argo CD.

### Directory Structure

|Directory|Description|
|:---|:---|
|ansible/|All Ansible Content|
|manifests/|Common Configuration|
|manifests/bootstrap|Deployment of OpenShift GitOps Operator and Argo CD|
|manifests/clusters/\<cluster-name\>|Cluster Specific Configuration|
|manifests/clusters/\<cluster-name\>/argocd/apps/\<cluster-name\>|Cluster Bootstrap|
|manifests/clusters/\<cluster-name\>/argocd/apps/cluster|Initial Base Application to Deploy other Applications|

## Applying Configuration

Argo CD will be deployed using the OpenShift GitOps operator. Install the operator as follows:

```shell
oc apply -k manifests/bootstrap/openshift-gitops-operator/base
```

Next deploy Argo CD using the OpenShift GitOps operator.

```shell
until oc apply -k manifests/bootstrap/openshift-gitops/base; do sleep 5; done
```

Finally, we can configure our cluster by deploying the base application (app of apps methodology). The base application will in turn deploy all other applications/configurations.

```shell
oc apply -k manifests/clusters/<cluster-name>/argocd/apps/<cluster-name>/base
```