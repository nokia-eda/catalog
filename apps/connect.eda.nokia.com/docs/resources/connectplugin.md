---
resource_name: ConnectPlugin
resource_name_plural: connectplugins
resource_name_plural_title: Connect Plugins
resource_name_acronym: CP
crd_path: docs/connect.eda.nokia.com/crds/connect.eda.nokia.com_connectplugins.yaml
---

# Connect Plugin

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

A `ConnectPlugin` is the logical plugin instance registered with Connect Core. Cloud Connect plugins create or update this object when they start with valid credentials.

## Custom Resource Definition

To browse the Custom Resource Definition go to [crd.eda.dev](https://crd.eda.dev/-{{ resource_name_plural }}-.-{{ app_group }}-/-{{ app_api_version }}-).

-{{ crd_viewer(crd_path, collapsed=True) }}-
