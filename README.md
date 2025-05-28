## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

  helm repo add rogly-net https://rogly-net.github.io/helm-charts

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
rogly-net` to see the charts.

To install the a chart:

    helm install <release-name> rogly-net/<chart-name>

To uninstall the chart:

    helm uninstall <release-name>

## Charts
- [UniFi-Exporter](https://github.com/rogly-net/helm-charts/blob/main/charts/unifi-exporter/README.md) : A Python based Loki Exporter for UniFi Controllers and Gateways
