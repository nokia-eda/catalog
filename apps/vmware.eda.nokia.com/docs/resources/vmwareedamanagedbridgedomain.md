---
resource_name: VmwareEDAManagedBridgeDomain
resource_name_plural: vmwareedamanagedbridgedomains
resource_name_plural_title: Vmware EDA Managed Bridge Domains
resource_name_acronym: VE
crd_path: docs/vmware.eda.nokia.com/crds/vmware.eda.nokia.com_vmwareedamanagedbridgedomains.yaml
---

# Vmware EDA Managed Bridge Domain

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

`VmwareEDAManagedBridgeDomain` associates a distributed port group with an existing EDA `BridgeDomain` for VMware EDA-managed mode.
This can be used instead of providing the association through metadata in vCenter itself.
## Dependencies

VMware plugin operator and a `VmwarePluginInstance` for the managing plugin.

## Referenced resources

EDA `BridgeDomain` and `ConnectPlugin` — see Cloud Connect documentation.

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
