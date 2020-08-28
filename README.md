# GihHub Self Hosted Runner

[kube-git-runner](https://github.com/kube-git-runner) Git Self Hosted Runner to Execute GitHub Actions workflows

## TL;DR

```console
$ helm repo add loki https://loki1978.github.io/kube-git-runner
$ helm install my-release loki/kube-git-runner
```

## Introduction

Charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0-beta3+

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm repo add kube-git-runner https://github.com/kube-git-runner
$ helm install my-release kube-git-runner/kube-git-runner
```

These commands deploy kube-git-runner on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release. Use the option `--purge` to delete all persistent volumes too.

## Parameters

The following tables lists the configurable parameters of the kube-git-runner chart and their default values.

### kube-git-runner parameters

| Parameter                              | Description                                                                 | Default                                                 |
|----------------------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------|
| `image.repository`                     | kube-git-runner image name                                                          | `loki1978/kube-git-runner`                                       |
| `image.tag`                            | kube-git-runner image tag                                                           | `{TAG_NAME}`                                            |
| `image.pullPolicy`                     | kube-git-runner image pull policy                                                   | `IfNotPresent`                                          |
| `image.pullSecrets`                    | Specify docker-registry secret names as an array                            | `[]` (does not add image pull secrets to deployed pods) |

### Deployment parameters

| Parameter                      | Description                                              | Default                        |
|--------------------------------|----------------------------------------------------------|--------------------------------|
| `replicaCount`                 | Number of kube-git-runner nodes                                  | `1`                            |
| `updateStrategy`               | Update strategy for the deployment                       | `{type: "RollingUpdate"}`      |
| `schedulerName`                | Alternative scheduler                                    | `nil`                          |
| `podLabels`                    | kube-git-runner pod labels                                       | `{}` (evaluated as a template) |
| `podAnnotations`               | kube-git-runner Pod annotations                                  | `{}` (evaluated as a template) |
| `affinity`                     | Affinity for pod assignment                              | `{}` (evaluated as a template) |
| `nodeSelector`                 | Node labels for pod assignment                           | `{}` (evaluated as a template) |
| `tolerations`                  | Tolerations for pod assignment                           | `[]` (evaluated as a template) |
| `livenessProbe`                | Liveness probe configuration for kube-git-runner                 | `Check values.yaml file`       |
| `readinessProbe`               | Readiness probe configuration for kube-git-runner                | `Check values.yaml file`       |
| `securityContext.enabled`      | Enable securityContext on for kube-git-runner deployment         | `true`                         |
| `securityContext.runAsUser`    | User for the security context                            | `1001`                         |
| `securityContext.fsGroup`      | Group to configure permissions for volumes               | `1001`                         |
| `securityContext.runAsNonRoot` | Run containers as non-root users                         | `true`                         |
| `resources.limits`             | The resources limits for kube-git-runner containers              | `{}`                           |
| `resources.requests`           | The requested resources for kube-git-runner containers           | `{}`                           |

### RBAC parameters

| Parameter                    | Description                                        | Default                                         |
|------------------------------|----------------------------------------------------|-------------------------------------------------|
| `serviceAccount.create`      | Enable creation of ServiceAccount for kube-git-runner pods | `true`                                          |
| `serviceAccount.name`        | Name of the created serviceAccount                 | Generated using the `kube-git-runner.fullname` template |
| `serviceAccount.annotations` | ServiceAccount Annotations                         | `{}`                                            |
### Exposure parameters

| Parameter                           | Description                                                                                                                                                                                                                           | Default             |
|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
| `service.type`                      | Kubernetes Service type                                                                                                                                                                                                               | `ClusterIP`         |
| `service.port`                      | kube-git-runner service port                                                                                                                                                                                                                  | `3000`              |
| `service.nodePort`                  | Port to bind to for NodePort service type (client port)                                                                                                                                                                               | `nil`               |
| `service.annotations`               | Annotations for kube-git-runner service                                                                                                                                                                                                       | `{}`                |
| `service.loadBalancerIP`            | loadBalancerIP if kube-git-runner service type is `LoadBalancer`                                                                                                                                                                              | `nil`               |
| `service.loadBalancerSourceRanges`  | loadBalancerSourceRanges if kube-git-runner service type is `LoadBalancer`                                                                                                                                                                    | `nil`               |
| `ingress.enabled`                   | Enable the use of the ingress controller to access the web UI                                                                                                                                                                         | `false`             |
| `ingress.annotations`               | Annotations for the kube-git-runner Ingress                                                                                                                                                                                                   | `{}`                |
| `ingress.hosts[0].name`             | Hostname to your kube-git-runner installation                                                                                                                                                                                                 | `kube-git-runner.local`     |
| `ingress.hosts[0].paths`            | Path within the url structure                                                                                                                                                                                                         | `["/"]`             |
| `ingress.hosts[0].extraPaths`       | Ingress extra paths to prepend to every host configuration. Useful when configuring [custom actions with AWS ALB Ingress Controller](https://kubernetes-sigs.github.io/aws-alb-ingress-controller/guide/ingress/annotation/#actions). | `[]`                |
| `ingress.hosts[0].tls`              | Utilize TLS backend in ingress                                                                                                                                                                                                        | `false`             |
| `ingress.hosts[0].tlsHosts`         | Array of TLS hosts for ingress record (defaults to `ingress.hosts[0].name` if `nil`)                                                                                                                                                  | `nil`               |
| `ingress.hosts[0].tlsSecret`        | TLS Secret (certificates)                                                                                                                                                                                                             | `kube-git-runner.local-tls` |
