# Standard Out Exporter

## Description

This EDA App writes network and EDA data to stdout. This enables data export that can be then forwarded to syslog using the eda-fluent or any other k8s logging infra.

## Getting started

After installing the App through the appstore, users can specify which sets of data are written using the **Export** Custom Resource (CR) or via the UI by navigating to **Standard Out Exporter**, **Exporters**.

## Examples

- Interfaces statistics:

```yaml
apiVersion: stdout.eda.nokia.com/v1alpha1
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
apiVersion: stdout.eda.nokia.com/v1alpha1
kind: Export
metadata:
  name: lldp-stats
spec:
  exports:
  - prefix: lldp
    path: .node.srl.system.lldp.interface.statistics
```