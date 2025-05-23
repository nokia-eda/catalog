# Kafka Exporter

## Description

This EDA App exports network and EDA notifications to a Kafka Server.
Users can specify which notifications are exported as well as an export mode one of: `periodic`, `on-change` or both (`periodic-on-change`).

## Getting started

After installing the App through the appstore, users can specify which notifications are exported using the **Producer** or **ClusterProducer** Custom Resources (CR) or via the UI by navigating to **Kafka** category.

## Examples

- Alarms

```yaml
apiVersion: kafka.eda.nokia.com/v1alpha1
kind: ClusterProducer
metadata:
  name: alarms
spec:
  brokers: kafka-service:9092
  exports:
    - path: .namespace.alarms.alarm
      topic: alarms
    - path: .namespace.alarms.current-alarm
      topic: alarms-current
    - path: .namespace.alarms.alarm.history
      topic: alarms-history
```

- Interfaces statistics and states:

```yaml
apiVersion: kafka.eda.nokia.com/v1alpha1
kind: ClusterProducer
metadata:
  name: interfaces
spec:
  brokers: kafka-service:9092
  exports:
    - path: .namespace.node.srl.interface.statistics
      topic: interfaces
      mode: periodic
      period: 10s
    - path: .namespace.node.srl.interface
      fields:
        - oper-state
        - admin-state
      topic: interfaces
      mode: on-change
```
