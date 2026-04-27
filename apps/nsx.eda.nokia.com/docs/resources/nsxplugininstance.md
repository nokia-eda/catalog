---
resource_name: NSXPluginInstance
resource_name_plural: nsxplugininstances
resource_name_plural_title: NSX Plugin Instances
resource_name_acronym: NP
crd_path: docs/nsx.eda.nokia.com/crds/nsx.eda.nokia.com_nsxplugininstances.yaml
---

# NSX Plugin Instance

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

An `NSXPluginInstance` configures the NSX Connect plugin: NSX Manager connectivity, credentials, related VMware plugin names, and heartbeat settings. NSX fabric automation requires matching vCenter plugins for `ConnectInterface` correlation.
It will result in a `ConnectPlugin` instance based on the given `externalID`.

## Dependencies

Cloud Connect core, VMware vSphere plugin app, and a Kubernetes `Secret` with NSX credentials.

## Referenced resources

`ConnectPlugin` and VMware `VmwarePluginInstance` objects — see Cloud Connect documentation.

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
