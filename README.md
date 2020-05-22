# R.Franco Techonologies Helm Charts

This repository contains Helm charts created by R.Franco Technologies.

## Strapi

In order to install the Strapi Helm chart from R.Franco Technologies first you have to add the chart repository to your helm installation:

```bash
$ helm repo add rfrancotechnologies https://rfrancotechnologies.github.io/helm-charts
$ helm repo update
```

Then, to install the chart with the release name `my-release`:

```bash
$ helm install my-release rfrancotechnologies/strapi
```

Have a look at the [Strapi README.md](https://github.com/rfrancotechnologies/helm-charts/blob/master/helm-chart-sources/strapi/README.md) for valid values for the chart.