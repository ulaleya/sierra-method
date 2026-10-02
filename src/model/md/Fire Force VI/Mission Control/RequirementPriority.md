---
template:
  id: https://fireforce6.github.io/mission-control/requirement-priority
  name: "Requirement Priority Analysis"
  rank: 0
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---

## Requirement Priority Summary

This reusable view summarizes the number of Requirements at each priority level.

```chart
---
type: bar
data:
  labels: priority
  datasets:
    - label: Requirements
      data: count
options:
  plugins:
    title:
      display: true
      text: Requirements by Priority
    legend:
      display: false
  scales:
    y:
      beginAtZero: true
---
PREFIX base: <https://www.modelware.io/sierra/base#>
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

SELECT ?priority (COUNT(?requirement) AS ?count)
WHERE {
    ?requirement a stakeholder:Requirement ;
                 base:priority ?priority .
}
GROUP BY ?priority
ORDER BY DESC(?count)
```