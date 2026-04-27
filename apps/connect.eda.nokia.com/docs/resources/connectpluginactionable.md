---
resource_name: ConnectPluginActionable
resource_name_plural: connectpluginactionables
resource_name_plural_title: Connect Plugin Actionables
resource_name_acronym: CA
crd_path: docs/connect.eda.nokia.com/crds/connect.eda.nokia.com_connectpluginactionables.yaml
---

# Connect Plugin Actionable

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

A `ConnectPluginActionable` represents work Connect Core requests from a plugin (for example triggering an audit). Plugins process actionables according to their supported actions.

## Referenced resources

The target `ConnectPlugin` and related Connect resources — see Cloud Connect documentation.

## Custom Resource Definition

To browse the Custom Resource Definition go to [crd.eda.dev](https://crd.eda.dev/-{{ resource_name_plural }}-.-{{ app_group }}-/-{{ app_api_version }}-).

-{{ crd_viewer(crd_path, collapsed=True) }}-
