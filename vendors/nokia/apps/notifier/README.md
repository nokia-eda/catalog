# Notifier

## Description

This EDA App creates and delivers custom notifications from a variety of sources to multiple destinations.

## Getting started

After installing the App from the app store, you can configure your notification sources and destinations.
You have the option to choose between two sources—Alarm or Query—and can send notifications to multiple destinations.
The full list of supported providers is available [here](https://containrrr.dev/shoutrrr/v0.8).

Sources are defined using the Notifier Custom Resource (CR), while destinations (referred to as Providers) are set up using the Provider CR.
You can mix and match sources, as well as send notifications to multiple destinations

## Examples

- Send all alarms to a Discord channel.

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: alarms-to-discord
  namespace: eda-system
spec:
  description: "Notifier for all alarms to Discord"
  enabled: true
  sources:
    alarms:
      include:
      - "*"
  providers:
  - discord
```

Provider CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
  namespace: eda-system
spec:
  enabled: true
  uri: discord://${TOKEN}@${WEBHOOKID}
```

- Send specific Alarms to Discord

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: interface-down-alarm-to-discord
  namespace: eda-system
spec:
  description: "Send InterfaceDown Alarm only"
  enabled: true
  sources:
    alarms:
      include:
      - InterfaceDown
  providers:
  - discord
```

Provider CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
  namespace: eda-system
spec:
  enabled: true
  uri: discord://${TOKEN}@${WEBHOOKID}
```

- Exclude certain alarms from being sent to Discord

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: alarms-to-discord
  namespace: eda-system
spec:
  description: "Notifier for all alarms except InterfaceDown to Discord"
  enabled: true
  sources:
    alarms:
      include:
      - "*"
      exclude:
      - "InterfaceDown"
  providers:
  - discord
```

Provider CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
  namespace: eda-system
spec:
  enabled: true
  uri: discord://${TOKEN}@${WEBHOOKID}
```

- Send custom notification based on an EQL query

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: new-lldp-neighbor
  namespace: eda-system
spec:
  description: "Notifier for new LLDP neighbors"
  enabled: true
  sources:
    query:
      title: "LLDP neighbor - new"
      table: .namespace.resources.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.status.members.neighbors
      fields:
      - .namespace.resources.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.name
      - interface
      - node
      template: "A new LLDP neighbor has appeared on interface {{index . \".namespace.resources.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.name\"}}: host name {{index . \"node\"}}, interface name {{index . \"interface\"}}"
  providers:
  - discord

```

Provider CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
  namespace: eda-system
spec:
  enabled: true
  uri: discord://${TOKEN}@${WEBHOOKID}
```

- Mixing sources

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: interface-down-to-discord
  namespace: eda-system
spec:
  description: "Notifier for interface down events"
  enabled: true
  sources:
    alarms:
      include:
      - InterfaceDown
      exclude:
      - InterfaceMemberDown
    query:
      table: .namespace.node.srl.interface
      where: oper-state = "down"
      fields:
      - .node.name
      - .node.srl.interface.name
      template: "🚨 {{ index . \".node.name\" }} has an interface down: {{ index . \"name\" }} 🚨"
  providers:
  - discord
  - teams
```

Provider CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
  namespace: eda-system
spec:
  enabled: true
  uri: discord://${TOKEN}@${WEBHOOKID}
---
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: teams
  namespace: eda-system
spec:
  enabled: true
  uri: teams://group@tenant/altid/groupowner
```
