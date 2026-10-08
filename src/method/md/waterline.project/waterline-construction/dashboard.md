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

The view supports construction review by bringing component identification and verification information together. A pending status indicates the verification status recorded in the model. It does not indpendently establish whether inspections or testing have been performed or completed in the field. The dashboard reflects only information represented in the model and should not be interpreted as a substitute for project inspection or testing records.

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

## Inspection and Testing Relationships

This table identifies the inspection and testing activities associated with each modeled waterline component. Blank values indicate that the corresponding relationship is not represented in the model.

```table
---
columns: { Component: { label: "Waterline Component" }, Status: { label: "Verification Status" }, Inspection: { label: "Required Inspection" }, Testing: { label: "Required Testing" } }
---
PREFIX waterline: <https://waterline.project/waterline-construction/waterline#>

SELECT ?Component ?Status ?Inspection ?Testing
WHERE {
    ?Component a waterline:WaterlineComponent .
    OPTIONAL { ?Component waterline:verificationStatus ?Status . }
    OPTIONAL { ?Component waterline:requiresInspection ?Inspection . }
    OPTIONAL { ?Component waterline:requiresTesting ?Testing . }
}
ORDER BY ?Component
```
  
  ## Waterline Construction Traceability Graph

This graph illustrates the modeled relationships among waterline requirements, components, installation activities, inspections, and testing activities. Disconnected elements may indicate missing traceability relationships.

```graph
---
expandOnClick: true
---
PREFIX waterline: <https://waterline.project/waterline-construction/waterline#>
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

CONSTRUCT {
    ?requirement waterline:appliesToComponent ?component .
    ?requirement waterline:appliesToActivity ?activity .
    ?requirement waterline:requiresVerification ?verification .
    ?component waterline:requiresInspection ?inspection .
    ?component waterline:requiresTesting ?testing .
    ?activity waterline:installs ?installedComponent .
}
WHERE {
    OPTIONAL {
        ?requirement a stakeholder:Requirement .
        OPTIONAL { ?requirement waterline:appliesToComponent ?component . }
        OPTIONAL { ?requirement waterline:appliesToActivity ?activity . }
        OPTIONAL { ?requirement waterline:requiresVerification ?verification . }
    }
    OPTIONAL {
        ?component a waterline:WaterlineComponent .
        OPTIONAL { ?component waterline:requiresInspection ?inspection . }
        OPTIONAL { ?component waterline:requiresTesting ?testing . }
    }
    OPTIONAL {
        ?activity a waterline:InstallationActivity .
        OPTIONAL { ?activity waterline:installs ?installedComponent . }
    }
}
```
  