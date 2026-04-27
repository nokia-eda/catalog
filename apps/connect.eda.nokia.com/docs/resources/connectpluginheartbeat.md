---
resource_name: ConnectPluginHeartbeat
resource_name_plural: connectpluginheartbeats
resource_name_plural_title: Connect Plugin Heartbeats
resource_name_acronym: CH
crd_path: docs/connect.eda.nokia.com/crds/connect.eda.nokia.com_connectpluginheartbeats.yaml
---

# Connect Plugin Heartbeat

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

Plugins send `ConnectPluginHeartbeat` objects at a configured interval so Connect Core can track liveness and raise alarms if heartbeats are missed.

## Dependencies

Connect Core and an active `ConnectPlugin`.

## Referenced resources

The corresponding `ConnectPlugin` — see Cloud Connect documentation.

## Custom Resource Definition

To browse the Custom Resource Definition go to [crd.eda.dev](https://crd.eda.dev/-{{ resource_name_plural }}-.-{{ app_group }}-/-{{ app_api_version }}-).

-{{ crd_viewer(crd_path, collapsed=True) }}-
