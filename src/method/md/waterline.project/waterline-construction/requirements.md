---
template:
  id: https://waterline.project/waterline-construction/requirements
  name: "Waterline Requirements"
  rank: 3
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# Waterline Requirements

Define the construction requirements that apply to waterline components and installation activities.

Use this pattern to document requirements and connect them to the physical components or installation activities they govern. These relationships provide traceability between what the project requires and the construction work being performed. High-priority requirements may be modeled as Critical Waterline Requirements when the methodology needs to distinguish requirements that require additional attention or verification.

```tree-editor
---
columns: { this: { label: "Requirement" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix stakeholder: <https://www.modelware.io/sierra/stakeholder#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:WaterlineRequirementShape
    a sh:NodeShape ;
    sh:targetClass stakeholder:Requirement ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path base:expression ;
        sh:name "Requirement Expression" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path base:priority ;
        sh:name "Priority" ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path waterline:appliesToComponent ;
        sh:name "Applies to Component" ;
        sh:class waterline:WaterlineComponent ;
    ] ;
    sh:property [
        sh:path waterline:appliesToActivity ;
        sh:name "Applies to Installation Activity" ;
        sh:class waterline:InstallationActivity ;
    ] ;
    .
```