# NATS Importer

NATS Importer is an EDA application that enables importing data from NATS servers into EDB. 
It supports both standard NATS messaging and JetStream persistent messaging with configurable delivery modes.

## Features

- **Dual Mode Support**: Works with both NATS Core messaging and JetStream
- **JetStream Push & Pull**: Supports both push and pull consumer modes for JetStream
- **Flexible Filtering**: Filter messages using subject patterns and CEL conditions
- **Data Transformation**: Transform message data using Go templates
- **TLS Support**: Secure connections with TLS configuration
- **Supports both namespace-scoped and cluster-scoped resources**: Allows you to manage Subscribers at the namespace level for fine-grained access, or use ClusterSubscribers for cluster-wide message import operations.

## Quick Start

1. Install the NATS Importer application through the EDA App Store
2. Create a Subscriber or ClusterSubscriber resource
3. Monitor the status to ensure successful connection and message processing