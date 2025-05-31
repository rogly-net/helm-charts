# UniFi Exporter Helm Chart

This Helm chart deploys [UniFi Exporter](https://github.com/rogly-net/unifi-exporter), a tool designed to export metrics from UniFi devices for monitoring purposes. The exporter does not expose a web interface but sends metrics to a configured monitoring system.

## Prerequisites

- Kubernetes cluster (v1.20+ recommended)
- Helm 3.0+
- [UniFi Gateway](https://store.ui.com/us/en?category=all-cloud-gateways)
- [Grafana Loki](https://github.com/grafana/loki)
- [MaxMind GeoLite](https://www.maxmind.com/en/geolite2/signup?utm_source=kb&utm_medium=kb-link&utm_campaign=kb-create-account)

## Installation

1. Add the Helm repository (if applicable):
   ```bash
   helm repo add rogly-net https://rogly-net.github.io/helm-charts
   ```

2. Install the chart:
   ```bash
   helm install test-release rogly-net/unifi-exporter --namespace observability
   ```

3. Verify the deployment:
   ```bash
   helm test test-release --namespace observability
   ```

## Customizing The Installation

The chart can be customized using the [`values.yaml`](https://github.com/rogly-net/helm-charts/blob/main/charts/unifi-exporter/values.yaml) file. Below are some key configuration options:

| Parameter                           | Description                                                 | Default               | Valid Values                                                       | Notes
|-------------------------------------|-------------------------------------------------------------|-----------------------|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| `deploymentMode`                    | Specifies the workload type to create                       | `statefulset`         | `statefulset` \| `deployment`                                      | `deployment` requires a `ReadWriteMany` capable `StorageClass` to be set for `persistence.database.storageClass`              |
| `config.logLevel`                   | Sets the console minimum log level                          | `informational`       | `debug` \| `informational` \| `warning` \| `error` \| `critical`   |                                                                                                                               |
| `config.timezone`                   | Configures the timezone used when writing timestamps        | `UTC`                 | `tz database`                                                      | [Acceptable Values](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List)                                        |
| `config.loki.url`                   | Configures the Loki endpoint to export logs to              | `http://loki:3100`    | `http(s)://hostname:port`                                          | When `loki.enabled` is set to `true` this defaults to `http://<release>-loki-gateway:<port>`                                  |
| `config.geoip.accountId`            | MaxMind Account ID                                          | `EmptyString`         | `Account_ID`                                                       | If not provided GeoIP encrichment will be disabled                                                                            |
| `config.geoip.licenseKey`           | MaxMind License Key                                         | `EmptyString`         | `License_Key`                                                      | If not provided GeoIP encrichment will be disabled                                                                            |
| `dashboards.enabled`                | Configures automatic provisioning of Grafana Dashboards     | `false`               | `true` \| `false`                                                  | Automatically provisions Grafana dashboards as `ConfigMaps` for the Grafana Dashboard Sidecar to pickup                       |
| `persistence.database.enabled`      | Configures GeoIP Database persistence                       | `true`                | `true` \| `false`                                                  | This is highly recommended to reduce API calls to MaxMind for container rebuilds and scaling                                  |
| `persistence.database.storageClass` | Configures the StorageClass used                            | `DefaultStorageClass` | `storageClassName `                                                | This is **required** if `deploymentMode` is `deployment` and your default `storageClass` does not support `ReadWriteMany`     |
| `persistence.configMaps`            | Overwrites configuration for enrichments                    | `EmptyJSON`           | `{ "Key": "Value", }`                                              | Only recommended for advanced users                                                                                           |
| `loki.enabled`                      | Enable the Loki SubChart                                    | `false`               | `true` \| `false`                                                  | This is recommended for *testing* UniFi Exporter <br> **Not Recommended for Production workloads**                            |
| `grafana.enabled`                   | Enable the Grafana SubChart                                 | `false`               | `true` \| `false`                                                  | This is recommended for *testing* UniFi Exporter <br> **Not Recommended for Production workloads**                            |

For a full list of configurable options, refer to the [`values.yaml`](https://github.com/rogly-net/helm-charts/blob/main/charts/unifi-exporter/values.yaml) file.

Apply the customizations: 
```bash
helm upgrade --install test-release rogly-net/unifi-exporter --namespace observability --values values.yaml
```

## Uninstallation

To uninstall the chart:
```bash
helm uninstall test-release --namespace observability
```
