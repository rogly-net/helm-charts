# UniFi Exporter Helm Chart

This Helm chart deploys the UniFi Exporter, a tool designed to export metrics from UniFi devices for monitoring purposes. The exporter does not expose a web interface but sends metrics to a configured monitoring system.

## Prerequisites

- Kubernetes cluster (v1.20+ recommended)
- Helm 3.0+
- [Grafana Loki](https://github.com/grafana/loki)
- [MaxMind GeoLite](https://www.maxmind.com/en/geolite2/signup?utm_source=kb&utm_medium=kb-link&utm_campaign=kb-create-account)

## Installation

1. Add the Helm repository (if applicable):
   ```bash
   helm repo add rogly-net https://rogly-net.github.io/helm-charts
   ```

2. Install the chart:
   ```bash
   helm install <release-name> rogly-net/unifi-exporter --namespace <namespace>
   ```

3. Verify the deployment:
   ```bash
   helm test <release-name> --namespace <namespace>
   ```

## Configuration

The chart can be customized using the `values.yaml` file. Below are some key configuration options:

| Parameter                          | Description                                                                                    | Notes                                                                                                   |
|------------------------------------|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| `deploymentMode`                   | Specifies whether to deploy as a `statefulset` or `deployment`.                                | Use `statefulset` for persistent workloads and `deployment` for stateless workloads.                    |
| `config.logLevel`                  | Sets the console minimum log level.                                                            | Common values are `info`, `warn`, `error`, and `debug`.                                                 |
| `config.timezone`                  | Configures the timezone used when writing timestamps.                                          | Example: `UTC`, `America/New_York`.                                                                     |
| `config.loki.url`                  | Configures the Loki endpoint to export logs to.                                                | Ensure the endpoint is reachable from the cluster.                                                      |
| `config.geoip.accountId`           | MaxMind Account ID (Required for GeoIP Enrichment).                                            | Obtain from your MaxMind account.                                                                       |
| `config.geoip.licenseKey`          | MaxMind License Key (Required for GeoIP Enrichment).                                           | Keep this key secure and avoid exposing it publicly.                                                    |
| `dashboards.enabled`               | Configures automatic provisioning of Grafana Dashboards.                                       | Set to `true` to enable dashboard provisioning.                                                         |
| `persistence.database.enabled`     | Configures database persistence (recommended to prevent hitting API limits).                   | Recommended for production environments.                                                                |
| `persistence.database.storageClass`| Configures the storage class used                                                              | Required if default storage class does not support `ReadWriteMany` and `deploymentMode` is `deployment` |
| `persistence.configMaps`           | Customizes configuration for enrichments                                                       | Recommended to configure `persistence.configMaps.portforwardRules` to match your environment)           |

For a full list of configurable options, refer to the [`values.yaml`](https://github.com/rogly-net/helm-charts/blob/main/charts/unifi-exporter/values.yaml) file.

## Uninstallation

To uninstall the chart:
```bash
helm uninstall <release-name> --namespace <namespace>
```
