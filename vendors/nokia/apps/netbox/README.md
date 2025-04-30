# EDA NetBox App

## Overview

This app integrates with NetBox using two custom resources (CRs):

- **Instance**: Defines the target NetBox instance to interact with.
- **Allocation**: Specifies the type of EDA allocation to create based on `NetboxPrefixes`.

> **Note**: Both CRs **must** be created in the same namespace (excluding `eda-system`).

---

## Prerequisites

### NetBox Configuration

To enable NetBox to send updates to the EDA app:

#### 1. Create a Webhook in NetBox

- **Name**: Any meaningful identifier
- **URL**:
  `https://${EDA_ADDR}:${EDA_PORT}/core/httpproxy/v1/netbox/webhook/${INSTANCE_NAMESPACE}/${INSTANCE_NAME}`
- **Method**: `POST`
- Leave all other settings as default.

#### 2. Create an Event Rule

- **Name**: Choose a relevant name
- **Objects**: Include **IPAM IPAddresses** and **IPAM Prefixes**
- **Enabled**: Yes
- **Event Types**:
  - Object Created
  - Object Updated
  - Object Deleted
- **Action**:
  - **Type**: Webhook
  - **Webhook**: Select the one created above

#### 3. API Token

- Generate a NetBox API token to be used by the app.

- The associated user **must** have permissions to `create`, `update`, and `delete` **IPAM.IPAddresses**, **IPAM.Prefixes**, **Customizations.Tags** and **Customizations.CustomFields** at a minimum.
---

## EDA Configuration

### Instance Custom Resource

Defines connection details to the NetBox instance:

```yaml
apiVersion: netbox.eda.nokia.com/v1alpha1
kind: Instance
metadata:
  name: netbox1
  namespace: eda
spec:
  apiToken: netbox-api-token # Secret containing the base64-encoded API token under key `apiToken`
  url: http://${NETBOX_ADDR}:${NETBOX_PORT}
```

After creation, check the status of the Instance CR to verify successful connection.

### Allocation Custom Resource

Defines what allocation to create, based on NetBox tags:

```yaml
apiVersion: netbox.eda.nokia.com/v1alpha1
kind: Allocation
metadata:
  name: ippool1
  namespace: eda
spec:
  enabled: true
  instance: netbox1  # <-- Reference to the Instance CR above
  tags:
    - eda-pool       # <-- Must match tags on NetBox prefixes
  type: ip-in-subnet # <-- One of: ip-address, subnet, ip-in-subnet
```

| `type`         | Resource Created                                           |
|----------------|------------------------------------------------------------|
| `ip-address`   | `ipallocationpools.core.eda.nokia.com`                     |
| `ip-in-subnet` | `ipinsubnetallocationpools.core.eda.nokia.com`            |
| `subnet`       | `subnetallocationpools.core.eda.nokia.com`                |
