# Introduction

This helm chart provides easy deployment of [Talaria](https://github.com/kelindar/talaria) on local, and can be adapted to work for any cloud provider.

# Pre-requisites

[Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) for this to work locally.
[Install Helm](https://helm.sh/docs/intro/install/) The following commands would work for Helm 3.0

# Installation

```sh
minikube start # Only for local
minikube mount PATH_TO_CONFIG:/config # Only for local

helm install talaria . -f values.yaml # The name of the chart for local needs to be talaria as minio endpoint is hardcoded in config.
```

# Cloud Providers

This helm chart is designed to work locally with minikube. To work with cloud, modify the components to fit your cloud requirement:  

## Storage Classes

The default storage class provisioner used is minikube provisioner. Add your storage class definition that is provided by [cloud providers](https://kubernetes.io/docs/concepts/storage/storage-classes/#the-storageclass-resource) accordingly. For example, to configure AWS EBS:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: talaria
provisioner: kubernetes.io/no-provisioner # Change according to cloud
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
parameters:
  type: gp2
```

Configure the `volumeClaimTemplates` in Talaria StatefulSet to reference this storage class.