# PagerDuty

This application creates PagerDuty incidents based on alarms and network events, streamlining incident management and escalation.

## Getting started

### Prerequisites

To authenticate with PagerDuty, you need a REST API key. Follow the [official documentation](https://support.pagerduty.com/main/docs/api-access-keys#section-generating-a-general-access-rest-api-key) to generate one.

The application requires the REST API key to be stored as a Kubernetes secret named eda-pd-api-key in the EDA base namespace (eda-system):

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: eda-pd-api-key
  namespace: eda-system
type: Opaque
data:
  key: <base64 of the REST API key>
```

Alerts are routed to a pre-configured PagerDuty [service](https://support.pagerduty.com/main/docs/services-and-integrations#section-events-API-v2). A service can represent a logical unit like a fabric, the entire network, or any structure that aligns with your escalation workflow.
Services are identified using routing (integration) keys. These keys must also be stored as Kubernetes secrets for reference during alert configuration:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: eda-pd-rkey
  namespace: eda-system
type: Opaque
data:
  key: <base64 of the routing key>
```

### Configuration

After installing the application from the app store, configure how alerts are sent to PagerDuty. Alerts can originate from two sources: Alarms or Queries.

To define the alert behavior, create a Pager custom resource (CR) in the EDA base namespace (eda-system). The Pager CR specifies a `routingKeySecret`, which determines the service that the alerts target.

## Examples

- Raise alerts based on all alarms

```yaml
apiVersion: pagers.eda.nokia.com/v1alpha1
kind: Pager
metadata:
  name: pagerduty-alarms-sample
  namespace: eda-system
spec:
  description: "Alarm based PagerDuty events"
  routingKeySecret: "eda-pd-rkey"
  sources:
    alarms:
      autoResolve: true
      include:
        - "*"
```

- Raise alerts based on alarms from namespace `eda`

```yaml
apiVersion: pagers.eda.nokia.com/v1alpha1
kind: Pager
metadata:
  name: pagerduty-alarms-sample
  namespace: eda-system
spec:
  description: "Alarm based PagerDuty events"
  routingKeySecret: "eda-pd-rkey"
  namespaces:
    - eda
  sources:
    alarms:
      autoResolve: true
      include:
        - "*"
```

- Raise an alert based on interfaces operational state.

```yaml
apiVersion: pagers.eda.nokia.com/v1alpha1
kind: Pager
metadata:
  name: pagerduty-query-sample
  namespace: eda-system
spec:
  description: "Query based PagerDuty events"
  routingKeySecret: eda-pd-rkey
  sources:
    query:
      autoResolve: true
      table: .namespace.node.srl.interface
      fields:
        - oper-state
        - admin-state
      where: oper-state = "disable"
      summary: 'Node {{ index . "node.name" }} has interface {{ index . "interface.name" }} oper-state {{ index . "oper-state" }} and admin-state {{ index . "admin-state" }}'
      source: '{{ index . "node.name" }}'
      severity: critical
      includeDetails: true
```
