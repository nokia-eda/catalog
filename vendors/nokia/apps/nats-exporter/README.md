# EDA NATS Exporter

## Description

This EDA app exports EDA data (such as alarms and query results) to external NATS or JetStream clusters, based on configured Export Custom Resources (`Export`, `ClusterExport`, `Publisher`, and `ClusterPublisher`).

## Integration

After installing the NATS Exporter application from the EDA app store, the first step is to configure how the application connects to your NATS or JetStream clusters.

### Publisher and ClusterPublisher Custom Resources

The app uses two types of publisher CRs to define NATS cluster connections:

- `Publisher` (per namespace)
- `ClusterPublisher` (cluster-wide, created in the EDA base namespace `eda-system`)

Each publisher resource specifies connection information such as cluster addresses, TLS settings, credentials, and publishing options.

The application will automatically establish and monitor connections to the clusters defined in these resources.

Example `Publisher`:

```yaml
apiVersion: nats.eda.nokia.com/v1alpha1
kind: Publisher
metadata:
  name: publisher1
  namespace: eda
spec:
  address: nats.example.com:4222
  type: NATS
  credentialsSecretName: nats-credentials # Kubernetes Secret with 'username' and 'password' keys
```

Example `ClusterPublisher`:

```yaml
apiVersion: nats.eda.nokia.com/v1alpha1
kind: ClusterPublisher
metadata:
  name: clusterpublisher1
  namespace: eda-system
spec:
  address: nats-cluster.example.com:4222
  type: Jetstream
  credentialsSecretName: nats-cluster-credentials
```

After creation, verify that the `status.connected` field is `true` to confirm a successful connection.

### NATS Credentials

If your NATS servers require authentication, the `credentialsSecretName` field in the Publisher CR must reference a Kubernetes Secret that contains two keys:

- `username`
- `password`

The application uses these credentials to authenticate against the NATS cluster.

TLS options can also be specified if your NATS server requires mutual TLS.

## Configuration

### Export and ClusterExport Custom Resources

Once a publisher is available, define what EDA data should be exported through Export and ClusterExport CRs.

- `Export` is namespace-scoped.

- `ClusterExport` is cluster-wide and must be created in the `eda-system` namespace.

Each export specifies:

- The data sources (alarms, queries)

- The destination NATS publishers and subjects

- Export modes (on-change, periodic)

Example Export:

```yaml
apiVersion: nats.eda.nokia.com/v1alpha1
kind: Export
metadata:
  name: alarms-export
  namespace: eda
spec:
  enabled: true
  destinations:
    - name: publisher1
      subject: eda.alarms
  exports:
    alarms:
      include:
        - *
```

Example `ClusterExport`:

```yaml
apiVersion: nats.eda.nokia.com/v1alpha1
kind: ClusterExport
metadata:
  name: interface-stats
  namespace: eda-system
spec:
  enabled: true
  destinations:
    - name: clusterpublisher1
      subjectFromJsPath: true
      subjectPrefix: eda
  exports:
    query:
      - path: .namespace.node.srl.interface.statistics
        mode: periodic
        period: 60
```

### Export Modes

When defining a query export, the following modes are available:

| mode       | Description                                  |
|------------|----------------------------------------------|
| on-change  | Export updates only when data changes        |
| periodic   | Export data at a fixed interval (`period`)   |
| both       | Export both on change and periodically       |

### Destination Subjects

Each destination specifies:

- The publisher name (which must match a created `Publisher` or `ClusterPublisher`)

- The subject to publish to on the NATS cluster

- Optionally, a dynamic subject generation based on the EDB data path (subjectFromJsPath) with an optional subjectPrefix.

### JetStream Subject Behavior

When publishing to a JetStream cluster, the application determines the target stream based on the subject name.

- If the subject contains a . (dot), the app applies the following logic:

    - It first checks if the first element (before the first dot) matches the name of an existing stream.

    - If a matching stream exists, the message is published to that stream, and the rest of the subject (after the first dot) is used as the message subject.

- If there is no exact match for the first element, the app tries to find which stream, based on subject filters, handles the given full subject.
