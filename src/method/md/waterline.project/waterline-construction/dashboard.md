---
template:
  id: https://waterline.project/waterline-construction/dashboard
  name: "Waterline Construction Dashboard"
  rank: 4
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# Waterline Construction Dashboard

This dashboard provides a live view of the waterline components represented in the project and their current verification status.

The view supports construction review by bringing component identification and verification information together. A pending status identifies modeled components for which verification has not yet been completed. The dashboard reflects only information represented in the model and should not be interpreted as a substitute for project inspection or testing records.

## Component Verification Status

```tree
---
columns: { this: { label: "Waterline Component" }, status: { label: "Verification Status" } }
---
PREFIX waterline: <https://waterline.project/waterline-construction/waterline#>

CONSTRUCT {
    ?component waterline:verificationStatus ?status .
}
WHERE {
    ?component a waterline:WaterlineComponent .
    OPTIONAL {
        ?component waterline:verificationStatus ?status .
    }
}
```