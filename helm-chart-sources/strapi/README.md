# strapi-helm-chart

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

- Kubernetes 1.10+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Adding the Helm Repository

```bash
$ helm repo add rfrancotechnologies https://rfrancotechnologies.github.io/helm-charts
helm repo update
```

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release rfrancotechnologies/strapi
```

The command deploys Strapi on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete --purge my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release completely.

## Configuration

The following table lists the configurable parameters of the MySQL chart and their default values.

| Parameter                                    | Description                                                                                  | Default                                              |
| -------------------------------------------- | -------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| `args`                                       | Additional arguments to pass to the MySQL container.                                         | `[]`                                                 |