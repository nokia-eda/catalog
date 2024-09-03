# Prometheus Exporter

## Description

This EDA App exports network and EDA metrics to Prometheus. Users can specify which metrics are exposed by the exporter for a Prometheus server to scrape.

## Getting started

After installing the App through the appstore, users can specify sets of metrics are exported using the **Export** Custom Resource (CR) or via the UI by navigating to **Exporters**, **Prometheus**.

## Examples

- Interfaces statistics:

```yaml
apiVersion: prom.eda.nokia.com/v1alpha1
kind: Export
metadata:
  name: interface-stats
spec:
  exports:
    - prefix: interfaces
      path: .node.srl.interface.statistics
```

- LLDP statistics:

```yaml
apiVersion: prom.eda.nokia.com/v1alpha1
kind: Export
metadata:
  name: lldp-stats
spec:
  exports:
  - prefix: lldp
    path: .node.srl.system.lldp.interface.statistics
```