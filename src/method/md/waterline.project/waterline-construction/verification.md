---
template:
  id: https://waterline.project/waterline-construction/verification
  name: "Waterline Verification"
  rank: 2
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# Waterline Inspection and Testing

Define the inspection and testing activities used to verify waterline construction.

Use this pattern to document the inspections and tests required for installed waterline components. Inspections and tests are modeled separately because they represent different forms of verification, but both provide evidence that construction requirements have been addressed. The specific verification activities required may vary depending on the component and project requirements.

## Inspections

```tree-editor
---
columns: { this: { label: "Inspection Activity" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:InspectionActivityShape
    a sh:NodeShape ;
    sh:targetClass waterline:InspectionActivity ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    .
```

## Testing

```tree-editor
---
columns: { this: { label: "Testing Activity" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:TestingActivityShape
    a sh:NodeShape ;
    sh:targetClass waterline:TestingActivity ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    .
```