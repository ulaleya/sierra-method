---
template:
  id: https://waterline.project/waterline-construction/installations
  name: "Waterline Installations"
  rank: 1
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# Waterline Installation Activities

Define the installation activities used to place waterline components into service.

Use this pattern to document an installation activity and identify the waterline component being installed. Connecting the activity to the component provides traceability between construction work and the physical asset. An installation should identify one component being installed so that later inspections, tests, and requirements can be evaluated in the context of that work.

```tree-editor
---
columns: { this: { label: "Installation Activity" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:InstallationActivityShape
    a sh:NodeShape ;
    sh:targetClass waterline:InstallationActivity ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path waterline:installs ;
        sh:name "Installs Component" ;
        sh:class waterline:WaterlineComponent ;
        sh:maxCount 1 ;
    ] ;
    .