---
resource_name: ConnectInterface
resource_name_plural: connectinterfaces
resource_name_plural_title: Connect Interfaces
resource_name_acronym: CI
crd_path: docs/connect.eda.nokia.com/crds/connect.eda.nokia.com_connectinterfaces.yaml
---

# Connect Interface

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

A `ConnectInterface` represents a hypervisor NIC or LAG. Plugins create these objects; Connect Core matches them to fabric `Interface` resources so the correct subinterfaces and overlay attachment are configured.

## Dependencies

EDA `Interface` resources on the fabric.

## Referenced resources

EDA `Interface` and related fabric objects — see Cloud Connect documentation.


///

## Custom Resource Definition

To browse the Custom Resource Definition go to [crd.eda.dev](https://crd.eda.dev/-{{ resource_name_plural }}-.-{{ app_group }}-/-{{ app_api_version }}-).

-{{ crd_viewer(crd_path, collapsed=True) }}-
