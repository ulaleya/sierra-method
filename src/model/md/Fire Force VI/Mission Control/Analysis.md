---
ontology: https://fireforce6.github.io/mission-control/bundle#
---

# Assignment 5: Fire Force Analysis
This analysis applies the Sierra Method analysis techniques to the Fire Force system description.
## 1. Conformance Analysis
### Engineering Question
Does every Stakeholder have at least one Concern associated with it? 

### Evidence

```table
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

SELECT ?stakeholder
    (IF(EXISTS {
        ?concern a stakeholder:Concern ;
            stakeholder:isExpressedBy ?stakeholder .
    }, "PASS", "FAIL") AS ?status)
WHERE {
        ?stakeholder a stakeholder:Stakeholder .
}
ORDER BY ?stakeholder
```

### Interpretation

All five Stakeholders in the Fire Force model have at least one associated Concern. Therefore, the model conforms to the tested rule. If a Stakeholder were added without an associated Concern, this analysis would identify that Stakeholder with a FAIL status. 

## 2. Near-Miss Analysis

### Engineering Question

Which Requirements are one priority level below High and therefore represent near misses to the High-priority condition? 

### Evidence

```table
PREFIX base: <https://www.modelware.io/sierra/base#>
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

SELECT ?requirement ?description ?priority
WHERE {
    ?requirement a stakeholder:Requirement ;
        base:description ?description ;
        base:priority ?priority .

    FILTER(?priority = "Medium")
}
ORDER BY ?requirement
```

### Interpretation

This analysis identifies Medium-priority Requirements as near misses to the defined High-priority condition. The result does not indicate that Medium-priority Requirements violate a Sierra rule; rather, it demonstrates how a query can identify model elements that are close to a specified analysis condition. 

## 3. Orphan Analysis

### Engineering Question

Are any Requirements missing a relationship to the Stakeholder that stated them? 

### Evidence

```table
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

SELECT ?requirement
WHERE {
    ?requirement a stakeholder:Requirement .

    FILTER NOT EXISTS {
        ?requirement stakeholder:isStatedBy ?stakeholder .
    }
}
ORDER BY ?requirement
```

### Interpretation

A Requirement returned by this query would be considered an orphan because it has no relationship to the Stakehholder that stated it. A clean result indicates that every Requirement in the Fire Force model is currently traced to a Stakeholder. 

## 4. Coverage Analysis

### Engineering Question

Which Stakeholders are associated with each Requirement, and where are there gaps in stakeholder-to-requirement coverage? 

### Evidence

```matrix
---
rowColumnLabel: Requirement / Stakeholder
---
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

SELECT ?row ?column (COALESCE(?n, 0) AS ?value)
WHERE {
    ?row a stakeholder:Requirement .
    ?column a stakeholder:Stakeholder .

    OPTIONAL {
        SELECT ?row ?column (COUNT(*) AS ?n)
        WHERE {
            ?row a stakeholder:Requirement ;
                stakeholder:isStatedBy ?column .
        }
        GROUP BY ?row ?column
    
    }
}
ORDER BY ?row ?column
```

### Interpretation

The matrix shows the relationship between Requirements and Stakeholders. A value of 1 indicates that the Stakeholder stated the Requirement, while a value of 0 indicates that no such relationship is present. The explicit zeros make the absence of stakeholder-to-requirement relationships visible rather than leaving missing relationships blank. These zeros show coverage patterns in the model but do not necessarily indicate a modeling deficiency.

## 5. Stakeholder-Requirement View Graph

### Engineering Question

How are Requirements connected to the Stakeholders who stated them? 

### Evidence

```graph
---
layout:
    mode: force
    running: true
    fit: true
    padding: 24
---
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

CONSTRUCT {
    ?stakeholder a stakeholder:Stakeholder .
    ?requirement a stakeholder:Requirement .
    ?requirement stakeholder:isStatedBy ?stakeholder .

}
WHERE {
    ?requirement a stakeholder:Requirement ;
        stakeholder:isStatedBy ?stakeholder .
    ?stakeholder a stakeholder:Stakeholder .
}
```

### Interpretation

The graph provides a visual view of the traceability between Requirements and the Stakeholders who stated them. It makes it easier to see how requirements are distributed across stakeholders and whether certain stakeholders are associated with more requirements than others. 

## 6. Scripted Requirement Priority Analysis

### Engineering Question

How are the Fire Force Requirements distributed across priority levels?

### Evidence

```python
include('src/method/py/utils.py')
import micropip
await micropip.install(['matplotlib', 'pandas'])
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
import pandas as pd

result = await query("""
    PREFIX base: <https://www.modelware.io/sierra/base#>
    PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>

    SELECT ?requirement ?priority
    WHERE {
        ?requirement a stakeholder:Requirement ;
                     base:priority ?priority .
    }
    ORDER BY ?priority
""")

rows = result['rows']

df = pd.DataFrame(rows)
counts = df['priority'].value_counts()

fig, ax = plt.subplots(figsize=(6, 4))
counts.plot(kind='bar', ax=ax)

ax.set_title('Requirements by Priority')
ax.set_xlabel('Priority')
ax.set_ylabel('Number of Requirements')
ax.tick_params(axis='x', rotation=0)

plt.tight_layout()
display(image_html(fig))
```

### Interpretation

The scripted analysis queries the Requirement priorities, computes the number of Requirements in each priority category, and renders the results as a bar chart. This provides a quick comparison of how the Fire Force Requirements are distributed across High, Medium, and Low priorities.

## 7. Reusable Requirement Priority View

The following analysis is rendered from a reusable compose template.

```compose
template: https://fireforce6.github.io/mission-control/requirement-priority
```

### Interpretation

The reusable view produces the same Requirement priority summary from a separate compose template. This demonstrates that the analysis can be reused in other pages while operating on the ontology context supplied by the calling page.