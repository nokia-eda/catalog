---
resource_name: ConnectAudit
resource_name_plural: connectaudits
resource_name_plural_title: Connect Audits
resource_name_acronym: CA
crd_path: docs/connect.eda.nokia.com/crds/connect.eda.nokia.com_connectaudits.yaml
---

# Connect Audit

-{{% import 'icons.html' as icons %}}-

-{{ category(resource_name_plural ) }}- → -{{ icons.circle(letter=resource_name_acronym, text=resource_name_plural_title) }}-

A `ConnectAudit` drives an operator-initiated reconcile between a Connect Plugin and EDA so fabric and cloud-reported state stay aligned. Status and results are recorded on the object.
An Audit could be necessary when connectivity has been temporarily interrupted between EDA and the Cloud Platform, due to an outage or an upgrade.

## Dependencies

Connect Core and the relevant plugins.

## Referenced resources

Resources inspected during the audit — see Cloud Connect documentation.

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
