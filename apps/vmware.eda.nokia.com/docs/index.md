# VMware vSphere Plugin Application

-{{% import 'icons.html' as icons %}}-

| <nbsp> {: .hide-th } |                                        |
| -------------------- |----------------------------------------|
| **Group/Version**     | -{{ app_group }}-/-{{ app_api_version }}-  |
| **Supported OS**     | -{{ supported_os_versions() }}- |
| **Catalog**          | [nokia-eda/catalog/vmware][manifest]      |
| **Source Code**      | <small>coming soon</small>             |

[manifest]: https://github.com/nokia-eda/catalog/blob/main/vendors/nokia/apps/vmware/manifest.yaml

The VMware vSphere plugin integrates VMware vCenter (distributed vSwitches and port groups) with Cloud Connect so fabric resources follow vCenter networking changes.

Full installation and behavior are documented on the [EDA documentation site](https://docs.eda.dev/) under apps > Cloud Connect. This app requires the Cloud Connect core application.

The application provides the following components:

/// tab | Resource Types

<div class="grid" markdown>
<div markdown>

* [VMware Plugin Instances](resources/vmwareplugininstance.md)

</div>
<div markdown>

* [VMware EDA Managed Bridge Domains](resources/vmwareedamanagedbridgedomain.md)

</div>
</div>
///
