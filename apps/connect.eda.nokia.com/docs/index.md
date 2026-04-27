# Cloud Connect Application

-{{% import 'icons.html' as icons %}}-

| <nbsp> {: .hide-th } |                                        |
| -------------------- |----------------------------------------|
| **Group/Version**     | -{{ app_group }}-/-{{ app_api_version }}-  |
| **Supported OS**     | -{{ supported_os_versions() }}- |
| **Catalog**          | [nokia-eda/catalog/connect][manifest]      |
| **Source Code**      | <small>coming soon</small>             |

[manifest]: https://github.com/nokia-eda/catalog/blob/main/vendors/nokia/apps/connect/manifest.yaml

Cloud Connect bridges the hypervisor and cloud control plane with the EDA fabric so fabric configuration can track compute and network state. Connect Core manages plugins, correlates `ConnectInterface` objects with fabric `Interface` resources, and coordinates audits.

Installation, architecture, and operations are documented on the [EDA documentation site](https://docs.eda.dev/).

The application provides the following components:

/// tab | Resource Types

<div class="grid" markdown>
<div markdown>

* [Connect Plugins](resources/connectplugin.md)
* [Connect Plugin Actionables](resources/connectpluginactionable.md)
* [Connect Plugin Heartbeats](resources/connectpluginheartbeat.md)

</div>
<div markdown>

* [Connect Interfaces](resources/connectinterface.md)
* [Connect Audits](resources/connectaudit.md)

</div>
</div>
///
