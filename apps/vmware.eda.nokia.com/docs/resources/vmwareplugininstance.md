---
resource_name: VmwarePluginInstance
resource_name_plural: vmwareplugininstances
resource_name_plural_title: Vmware Plugin Instances
resource_name_acronym: VP
crd_path: docs/vmware.eda.nokia.com/crds/vmware.eda.nokia.com_vmwareplugininstances.yaml
---

# Vmware Plugin Instance

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

A `VmwarePluginInstance` configures one VMware vSphere Connect plugin: vCenter connectivity, credentials, heartbeat options, and the plugin `externalID` used when registering with Connect Core.
It will result in a `ConnectPlugin` instance based on the given `externalID`.

## Dependencies

Cloud Connect core application and a Kubernetes `Secret` with vCenter credentials.

## Referenced resources

`ConnectPlugin` and related Connect resources created by the plugin — see Cloud Connect documentation.

## Examples

/// tab | YAML

```yaml
-{{ include_snippet(resource_name) }}-
```

///

/// tab | `kubectl`

```bash
cat << 'EOF' | kubectl apply -f -
-{{ include_snippet(resource_name) }}-
EOF
```

///

## Custom Resource Definition

To browse the Custom Resource Definition go to [crd.eda.dev](https://crd.eda.dev/-{{ resource_name_plural }}-.-{{ app_group }}-/-{{ app_api_version }}-).

-{{ crd_viewer(crd_path, collapsed=True) }}-
