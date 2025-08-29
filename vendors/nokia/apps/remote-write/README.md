# Remote Write

## Description

This EDA App exports network and EDA metrics to Remote Server. Users can specify which metrics are exposed by the exporter for a Remote Server.

## Getting started

After installing the App through the appstore, users can specify which sets of metrics are exported using the **Export** Custom Resource (CR) or via the UI by navigating to **RemoteWrite**, **Exporters**.

## Examples

- Interfaces statistics:

```yaml
apiVersion: remotewrite.eda.nokia.com/v1alpha1
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
apiVersion: remotewrite.eda.nokia.com/v1alpha1
kind: Export
metadata:
  name: lldp-stats
spec:
  exports:
  - prefix: lldp
    path: .node.srl.system.lldp.interface.statistics
```
