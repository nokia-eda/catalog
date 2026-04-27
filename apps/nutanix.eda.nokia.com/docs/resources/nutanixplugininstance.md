---
resource_name: NutanixPluginInstance
resource_name_plural: nutanixplugininstances
resource_name_plural_title: Nutanix Plugin Instances
resource_name_acronym: NP
crd_path: docs/nutanix.eda.nokia.com/crds/nutanix.eda.nokia.com_nutanixplugininstances.yaml
---

# Nutanix Plugin Instance

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

A `NutanixPluginInstance` configures the Nutanix Prism Central Connect plugin: API endpoint, credentials, and plugin settings.
It will result in a `ConnectPlugin` instance based on the given `externalID`.

## Dependencies

Cloud Connect core and a Kubernetes `Secret` with Prism Central credentials.

## Referenced resources

`ConnectPlugin` and related Connect resources — see Cloud Connect documentation.

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
