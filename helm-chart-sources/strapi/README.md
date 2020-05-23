# Strapi Helm Chart

Helm chart for the Strapi CMS.

It allows to deploy your own Docker image of Strapi with the following features:

* Ability to provide custom environment variables to your Strapi Docker image from Helm values.
* Ability to provide custom secrets, as environment variables to your Strapi Docker image from Helm values.
* Optionally create a PersistentVolumeClaim and volume mount for storing the uploaded media.
* Ability to start several instances of Strapi, with load balancing of requests.
* Expose Strapi behind an ingress (reverse proxy, potentially with HTTPs). 
* Liveness probe for detecting that a pod of Strapi stopped working and restarting it based on an HTTP probe.
* Readiness probe for detecting that a pod of Strapi is not working and taking it out of the load balancing of requests, based on an HTTP probe.

**Warning**: Do not use `sqlite` as database for Strapi. In that case each pod of Strapi will have its own database and content, and all your content will be deleted every time a pod is deleted. You must use one of the production-ready databases Strapi supports: Mysql, Postgresql or MongoDB.

## Prerequisites

- Kubernetes 1.16+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Adding the Helm Repository

```bash
$ helm repo add rfrancotechnologies https://rfrancotechnologies.github.io/helm-charts
$ helm repo update
```

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release rfrancotechnologies/strapi
```

The command deploys Strapi on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release completely.

## Configuration

The following table lists the configurable parameters of the Strapi chart and their default values.

| Parameter                            | Description                                                                                                                                                                         | Default               |   |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|---|
| `replicaCount`                       | Number of replicas to start                                                                                                                                                         | `1`                   |   |
| `image.repository`                   | The Docker image to start.                                                                                                                                                          | `strapi/strapi`       |   |
| `image.tag`                          | Image tag                                                                                                                                                                           | `3.0.0-beta.20.3`     |   |
| `image.pullPolicy`                   | Image pull policy                                                                                                                                                                   | `IfNotPresent`        |   |
| `service.type`                       | Desired service type                                                                                                                                                                | `ClusterIP`           |   |
| `service.port`                       | Service exposed port                                                                                                                                                                | `1337`                |   |
| `serviceAccount.create`              | Should we create a ServiceAccount                                                                                                                                                   | `true`                |   |
| `serviceAccount.name`                | Name of the ServiceAccount to use                                                                                                                                                   | `null`                |   |
| `ingress.enabled`                    | Enable or disable the ingress                                                                                                                                                       | `false`               |   |
| `ingress.hosts`                      | The virtual host name(s)                                                                                                                                                            | `{}`                  |   |
| `ingress.annotations`                | An array of service annotations                                                                                                                                                     | `nil`                 |   |
| `ingress.tls[i].secretName`          | The secret kubernetes.io/tls                                                                                                                                                        | `nil`                 |   |
| `ingress.tls[i].hosts[j]`            | The virtual host name                                                                                                                                                               | `nil`                 |   |
| `resources`                          | Resources allocation (Requests and Limits)                                                                                                                                          | `{}`                  |   |
| `env`                                | Environment variables that will be passed to the Docker image. They must be provided in the form `KEY: VALUE`.                                                                      | `{}`                  |   |
| `env_secrets`                        | Secrets that will be provided as environment variables to the Docker image. They must be provided in the form `KEY: VALUE`.                                                         | `{}`                  |   |
| `persistence.enabled`                | Enable config persistence using PVC                                                                                                                                                 | `true`                |   |
| `persistence.storageClass`           | PVC Storage Class for config volume                                                                                                                                                 | `nil`                 |   |
| `persistence.existingClaim`          | Name of an existing PVC to use for config                                                                                                                                           | `nil`                 |   |
| `persistence.accessMode`             | PVC Access Mode for config volume                                                                                                                                                   | `ReadWriteOnce`       |   |
| `persistence.size`                   | PVC Storage Request for config volume                                                                                                                                               | `8Gi`                 |   |
| `persistence.uploadsMountPath`       | Path on wich the persisten volume for uploaded media must be mounted.                                                                                                               | `/app/public/uploads` |   |
| `resources`                          | Resource limits for Quassel pod                                                                                                                                                     | `{}`                  |   |
| `nodeSelector`                       | Map of node labels for pod assignment                                                                                                                                               | `{}`                  |   |
| `tolerations`                        | List of node taints to tolerate                                                                                                                                                     | `[]`                  |   |
| `affinity`                           | Map of node/pod affinities                                                                                                                                                          | `{}`                  |   |
| `livenessProbe.initialDelaySeconds`  | Number of seconds after the container has started before liveness probes are initiated.                                                                                             | `120`                 |   |
| `livenessProbe.periodSeconds`        | How often (in seconds) to perform the probe.                                                                                                                                        | `10`                  |   |
| `livenessProbe.failureThreshold`     | When a Pod starts and the probe fails, Kubernetes will try failureThreshold times before giving up. Giving up in case of liveness probe means restarting the container.             | `3`                   |   |
| `readinessProbe.initialDelaySeconds` | Number of seconds after the container has started before readiness probes are initiated.                                                                                            | `0`                   |   |
| `readinessProbe.periodSeconds`       | How often (in seconds) to perform the probe.                                                                                                                                        | `10`                  |   |
| `readinessProbe.failureThreshold`    | When a Pod starts and the probe fails, Kubernetes will try failureThreshold times before giving up. Giving up in case of readiness probe means that the Pod will be marked Unready. | `3`                   |   |
