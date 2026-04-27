# VMware NSX Plugin Application

-{{% import 'icons.html' as icons %}}-

| <nbsp> {: .hide-th } |                                        |
| -------------------- |----------------------------------------|
| **Group/Version**     | -{{ app_group }}-/-{{ app_api_version }}-  |
| **Supported OS**     | -{{ supported_os_versions() }}- |
| **Catalog**          | [nokia-eda/catalog/nsx][manifest]      |
| **Source Code**      | <small>coming soon</small>             |

[manifest]: https://github.com/nokia-eda/catalog/blob/main/vendors/nokia/apps/nsx/manifest.yaml

The VMware NSX plugin integrates NSX segments and transport configuration with Cloud Connect. It depends on matching VMware vCenter plugins for host and interface correlation.

Full installation and behavior are documented on the [EDA documentation site](https://docs.eda.dev/) under apps > Cloud Connect.

The application provides the following components:

/// tab | Resource Types

<div class="grid" markdown>
<div markdown>

* [NSX Plugin Instances](resources/nsxplugininstance.md)

</div>
</div>
///
