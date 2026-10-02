---
template:
  id: https://waterline.project/waterline-construction/components
  name: "Waterline Components"
  rank: 0
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---

## Pipes

```tree-editor
---
columns: { this: { label: "Pipe" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:PipeShape
    a sh:NodeShape ;
    sh:targetClass waterline:Pipe ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path waterline:verificationStatus ;
        sh:name "Verification Status" ;
        sh:maxCount 1 ;
    ] ;
    .
```

## Fittings

```tree-editor
---
columns: { this: { label: "Fitting" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:FittingShape
    a sh:NodeShape ;
    sh:targetClass waterline:Fitting ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path waterline:verificationStatus ;
        sh:name "Verification Status" ;
        sh:maxCount 1 ;
    ] ;
    .
```

## Valves

```tree-editor
---
columns: { this: { label: "Valve" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:ValveShape
    a sh:NodeShape ;
    sh:targetClass waterline:Valve ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path waterline:verificationStatus ;
        sh:name "Verification Status" ;
        sh:maxCount 1 ;
    ] ;
    .
```

## Hydrants

```tree-editor
---
columns: { this: { label: "Hydrant" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix waterline: <https://waterline.project/waterline-construction/waterline#> .

waterline:HydrantShape
    a sh:NodeShape ;
    sh:targetClass waterline:Hydrant ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
    ] ;
    sh:property [
        sh:path waterline:verificationStatus ;
        sh:name "Verification Status" ;
        sh:maxCount 1 ;
    ] ;
    .
```