# Remote Write

## Description

This EDA App exports network and EDA metrics to Remote Server. Users can specify which metrics are exposed by the exporter for a Remote Server.

## Getting started

After installing the App through the appstore, users can specify which sets of metrics are exported using the **Export**, **ClusterExport**, **Destination** and **ClusterDestination** Custom Resources (CRs) or via the UI by navigating to **Remote Write**.

## Examples

- Destination:

```yaml
apiVersion: remotewrite.eda.nokia.com/v1alpha1
kind: Destination
metadata:
  name: dest1
  namespace: eda
spec:
    url: 'http://prw.example.com:9090/api/v1/write'
    writeOptions:
      bufferSize: 100
      flushInterval: 60s
```

- Interfaces statistics:

```yaml
apiVersion: remotewrite.eda.nokia.com/v1alpha1
kind: Export
metadata:
  name: interface-stats
  namespace: eda
spec:
  exports:
    - prefix: interfaces
      path: .namespace.node.srl.interface.statistics
  destinations:
    - name: dest1
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
    path: .namespace.node.srl.system.lldp.interface.statistics
  destinations:
    - name: dest1
```
