# Notifier

## Description

This EDA App creates and delivers custom notifications from a variety of sources to multiple destinations.

## Getting started

After installing the App from the app store, you can configure your notification sources and destinations.
You have the option to choose between two sources—Alarm or Query—and can send notifications to multiple destinations.
The full list of supported providers is available [here](https://containrrr.dev/shoutrrr/v0.8).

Sources are defined using the Notifier (or ClusterNotifier) Custom Resource (CR), while destinations (referred to as Providers ) are set up using the Provider or ClusterProvider CRs.
You can mix and match sources and send notifications to multiple destinations

## Resources

### Notifier and ClusterNotifier

The Notifier resource can be created in any namespace except the EDA base namespace (`eda-system`) while the ClusterNotifier can only be created in the EDA base namespace. ClusterNotifier can notify on sources across all namespaces while a Notifier sources notifications from its namespace only.

### Provider and ClusterProvider

The Provider resource can be created in any namespace except the EDA base namespace (`eda-system`) while the ClusterProvider can only be created in the EDA base namespace. ClusterProviders can only be referenced by ClusterNotifier resources while Providers can only be referenced by Notifier resources.

## Notification sources

### Alarm

Alarms can be used as a source for notifications, the user can set the list of included alarm types as well as a list of excluded types. Whenever an Alarm with an included type is raised, a notification is generated and send using the referenced providers.
The format of the Alarm-based notification depends on the provider type.

- **Notifier**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: alarms-to-discord
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

- **ClusterNotifier**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: ClusterNotifier
metadata:
  name: alarms-to-discord
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

### Query

EQL queries can be used as a source for notifications. Users can specify the table, select relevant fields, and define conditions to trigger a notification. When the condition is met, a notification is generated and sent using the referenced providers.

The notification supports two fields for customization: title and template, both of which use Go templates. These templates render based on a map that includes all selected fields and the keys returned by the table. The key names match the raw column names shown in the EDA UI query tool.

For example, querying the table `.namespace.node.srl.interface` returns the keys:

- `.namespace.name`
- `.namespace.node.name`
- `name`

And of course any field that was explicitly requested under `fields`. These can then be used within your template to dynamically construct notification messages.

- **Notifier**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: interface-down-notifier
spec:
  enabled: true
  sources:
    query:
      fields:
        - admin-state
        - oper-state
        - .namespace.node.name
      table: .node.srl.interface
      template: >-
        Namespace: {{ index . ".namespace.name" }}.\nInterface {{ index . "name"}} is DOWN on node {{ index .
        ".namespace.node.name"}} (admin state:  {{ index . "admin-state" }})
      title: Interface Down Alert
      where: admin-state ="enable" and oper-state = "down"
  providers:
    - teams
```

- **ClusterNotifier**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: ClusterNotifier
metadata:
  name: interface-down-notifier
spec:
  enabled: true
  sources:
    query:
      fields:
        - admin-state
        - oper-state
        - .namespace.node.name
      table: .namespace.node.srl.interface
      template: >-
        Interface {{ index . "name"}} is DOWN on node {{ index .
        ".namespace.node.name"}} (State:  {{ index . "oper-state" }})
      title: Interface Down Alert
      where: admin-state ="enable" and oper-state = "down"
  providers:
    - teams
```

## Notification destinations

Notifier supports multiple notification providers via [the shoutrrr external library](https://containrrr.dev/shoutrrr/v0.8) as well as custom implementations for `teams`, `discord`, `slack` and `email` providers.

### Teams

The `teams` provider allows users to send **MS Teams** notifications when events occur in the network or within EDA.
The integration is done using **Teams** `Incoming Webhook Connector`, a guide can be found [here](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook?tabs=newteams%2Cdotnet).

Copy the generated webhook address and replace the `https://` scheme with `teams://` to configure the teams provider.

- **Provider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: teams
spec:
  enabled: true
  uri: teams://${SCHEMELESS_WEBHOOK_URL}
```

- **ClusterProvider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: ClusterProvider
metadata:
  name: teams
spec:
  enabled: true
  uri: teams://${SCHEMELESS_WEBHOOK_URL}
```

### Discord

The `discord` provider allows users to send **Discord** notifications when events occur in the network or within EDA.
The integration is done using **Discord** webhooks, a guide can be found [here](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks).

Copy the generated webhook address and replace the `https://` scheme with `discord://` to configure the discord provider.

The Discord username posting the notification can optionally be customized using a query parameter `username` overwriting the default webhook bot name.
e.g : `discord://discord.com/api/webhooks/123456789/XXXXXXXXXXXXX?username=EDA`

- **Provider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
spec:
  enabled: true
  uri: discord://${SCHEMELESS_WEBHOOK_URL}
```

- **ClusterProvider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: ClusterProvider
metadata:
  name: discord
spec:
  enabled: true
  uri: discord://${SCHEMELESS_WEBHOOK_URL}
```

### Slack

The `slack` provider allows users to send **Slack** notifications when events occur in the network or within EDA.
The integration is done using **Slack** webhooks, a guide can be found [here](https://api.slack.com/messaging/webhooks).

Copy the generated webhook address and replace the `https://` scheme with `slack://` to configure the slack provider.

The Slack channel where the notification must be posted as well as the username posting it can optionally be customized using a query parameters `channel` and `username` overwriting the default webhook name and destination channel.
E.g : `slack://hooks.slack.com/services/XXXXX/YYYYYY/ZZZZZZ?username=EDA&channel=alerts`

- **Provider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: slack
spec:
  enabled: true
  uri: slack://${SCHEMELESS_WEBHOOK_URL}
```

- **ClusterProvider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: ClusterProvider
metadata:
  name: slack
spec:
  enabled: true
  uri: slack://${SCHEMELESS_WEBHOOK_URL}
```

### Email

The `email` provider allows users to send notifications as emails when events occur in the network or within EDA.
Notifier sends an email given an SMTP address and some additional parameters:

The SMTP address must start with `smtp://`.
If a username and password are required they must be part of the URI authority field `smtp://$user:$password@host`
Additional query parameters can be added to the URI:

- `from`     : The sender email address
- `to`       : The recipient email address
- `startTLS` : yes|no, if set to yes  the connection to the SMTP server must use TLS.
- `useHTML`  : yes|no, if yes the email content type will be set to "text/html; charset=UTF-8" otherwise "text/plain; charset=UTF-8"

- **Provider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: email
spec:
  enabled: true
  uri: smtp://$user:$password@host?from=eda@nokia.com&to=noc@customer.com&startTLS=yes
```

- **ClusterProvider**

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: ClusterProvider
metadata:
  name: email
spec:
  enabled: true
  uri: smtp://$user:$password@host?from=eda@nokia.com&to=noc@customer.com&startTLS=yes
```

### Others

Additional Providers are supported via the [shoutrrr](https://containrrr.dev/shoutrrr/v0.8) library.

## Proxy settings

If the Notifier app is sitting behind a proxy it can be configured using environment variables `HTTP_PROXY`, `HTTPS_PROXY` and `NO_PROXY`.

## Examples

- Send all alarms to a Discord channel.

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: alarms-to-discord
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
  name: filtered-to-discord
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
spec:
  enabled: true
  uri: discord://${SCHEMELESS_WEBHOOK_URL}
```

- Exclude certain alarms from being sent to Discord

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: alarms-to-discord
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
spec:
  enabled: true
  uri: discord://${SCHEMELESS_WEBHOOK_URL}
```

- Send custom notification based on an EQL query

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: interface-down-notifier
spec:
  enabled: true
  sources:
    query:
      fields:
        - admin-state
        - oper-state
        - .namespace.node.name
      table: .node.srl.interface
      template: >-
        Interface {{ index . "name"}} is DOWN on node {{ index .
        ".namespace.node.name"}} (State:  {{ index . "oper-state" }})
      title: Interface Down Alert
      where: admin-state ="enable" and oper-state = "down"
  providers:
    - discord
```

Provider CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: discord
spec:
  enabled: true
  uri: discord://${SCHEMELESS_WEBHOOK_URL}
```

- Mixing sources

Notifier CR

```yaml
apiVersion: notifiers.eda.nokia.com/v1
kind: Notifier
metadata:
  name: interface-down-to-discord
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
      table: .node.srl.interface
      where: oper-state = "down"
      fields:
      - .node.name
      - .interface.name
      template: "🚨 {{ index . \".namespace.node.name\" }} has an interface down: {{ index . \"name\" }} 🚨"
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
spec:
  enabled: true
  uri: discord://${SCHEMELESS_WEBHOOK_URL}
---
apiVersion: notifiers.eda.nokia.com/v1
kind: Provider
metadata:
  name: teams
spec:
  enabled: true
  uri: teams://${SCHEMELESS_WEBHOOK_URL}
```
