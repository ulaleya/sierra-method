---
template:
  id: https://www.modelware.io/sierra/system-analysis/ports
  name: "Ports"
  rank: 0
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# Ports

Define component ports and their directions for system interfaces.

```table-editor
---
columns: { this: { label: "Port" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix component: <https://www.modelware.io/sierra/component#> .

component:PortShape
    a sh:NodeShape ;
    sh:targetClass component:Port ;
    sh:property [
        sh:path component:direction ;
        sh:name "Direction" ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "Each port must have exactly one direction: In or Out." ;
    ] ;
    .