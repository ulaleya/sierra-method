---
ontology: https://waterline.project/waterline-construction/waterline-description
---

# Waterline Construction Analysis

## Verification Relationship Coverage

How many waterline components have modeled inspection and testing relationships?

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
    PREFIX waterline: <https://waterline.project/waterline-construction/waterline#>

    SELECT ?component ?inspection ?testing
    WHERE {
        ?component a waterline:WaterlineComponent .
        OPTIONAL {
            ?component waterline:requiresInspection ?inspection .
        }
        OPTIONAL {
            ?component waterline:requiresTesting ?testing .
        }
    }
""")

rows = result['rows']
df = pd.DataFrame(rows)

if not df.empty:
    coverage = df.groupby('component').agg(
        HasInspection=('inspection', lambda x: x.notna().any() and (x != '').any()),
        HasTesting=('testing', lambda x: x.notna().any() and (x != '').any())
    )
    complete = int((coverage['HasInspection'] & coverage['HasTesting']).sum())
    total = len(coverage)
    missing = total - complete
else:
    complete = 0
    total = 0
    missing = 0

print(f"Total waterline components: {total}")
print(f"Components with both relationships: {complete}")
print(f"Components missing one or both: {missing}")
print(f"Relationship coverage: {complete / total * 100:.1f}%" if total else "No components found")

fig, ax = plt.subplots(figsize=(6, 4))
ax.bar(['Both modeled', 'Missing one or both'], [complete, missing])
ax.set_title('Waterline Verification Relationship Coverage')
ax.set_ylabel('Number of Components')
ax.set_ylim(0, max(total + 1, 1))
plt.tight_layout()
display(image_html(fig))
```

### Interpretation

This scripted analysis measures whether waterline components have modeled inspection and testing relationships. The results identify gaps in model traceability, not evidence of incomplete field inspections or testing. A component is counted as having both relationships only when at least one inspection and one testing relationship are present.
  
  ## Methodology Question Findings

### Question 1: Requirement Applicability

The SPARQL analysis identified `BeddingRequirement_01` as applicable to `PipeSegment_01` and `PipeInstallation_01`. This demonstrates that the model can identify construction requirements associated with specific components and installation activities.

**Finding:** Requirement applicability is demonstrated for the pipe installation, but the other modeled components do not have associated requirements in the query results.

### Question 2: Required Inspection and Testing

The analysis identified `BeddingInspection_01` and `PressureTest_01` as required verification activities for `PipeSegment_01`. Four other components—`Fitting_01`, `Fitting_02`, `Valve_01`, and `Hydrant_01`—have no modeled inspection or testing relationships.

**Finding:** Verification relationship coverage is incomplete across the modeled components.

### Question 3: Requirement Change Impact

The analysis traced `BeddingRequirement_01` to `PipeSegment_01`, `PipeInstallation_01`, `BeddingInspection_01`, and `PressureTest_01` through direct and multi-step relationships.

**Finding:** The model supports identification of potentially affected elements when a requirement changes, but does not automatically determine whether changes to those elements are necessary.

### Question 4: Missing Verification

All five modeled components have `Pending` verification status. Only `PipeSegment_01` has both inspection and testing relationships. The model does not establish whether installation or verification activities have actually been completed.

**Finding:** The model detects missing verification relationships but cannot determine which installed components have outstanding field verification.

### Question 5: End-to-End Traceability

`BeddingRequirement_01` has modeled relationships to `PipeSegment_01`, `PipeInstallation_01`, `BeddingInspection_01`, and `PressureTest_01`.

**Finding:** The model demonstrates requirement-to-verification relationship traceability for the single modeled requirement. However, it does not contain completion evidence proving that the required verification activities were performed.

## Gap-Detection Findings

### Gap 1: Orphaned Installation Activities

A SPARQL query using `FILTER NOT EXISTS` identified two installation activities without `installs` relationships:

- `HydrantInstallation_01`
- `ValveInstallation_01`

**Classification:** Modeling pattern gap (Module 4).

**Recommendation:** Establish a modeling requirement that installation activities identify the components they install.

### Gap 2: Missing Verification Relationships

A second SPARQL query identified four components without `requiresInspection` or `requiresTesting` relationships:

- `Fitting_01`
- `Fitting_02`
- `Hydrant_01`
- `Valve_01`

**Classification:** Modeling pattern gap (Module 4).

**Recommendation:** Establish consistent relationships between waterline components and their required inspection and testing activities, where applicable.

### Gap 3: Missing Completion Evidence

The vocabulary provides a component-level `verificationStatus` property, but the analyzed model does not provide separate installation-completion or inspection/testing-completion records.

**Classification:** Vocabulary gap (Module 2) and potential modeling pattern gap (Module 4).

**Recommendation:** Consider adding installation and verification completion records in a future methodology iteration.

## Overall Interpretation

The Waterline Construction methodology successfully demonstrates requirements applicability, construction activity relationships, verification traceability, and model-based gap detection.

The analysis identified incomplete relationships affecting installation and verification traceability. The scripted computation measured verification relationship coverage across five modeled components.

These findings demonstrate the value of model-based systems engineering for reviewing construction information while identifying opportunities to improve the methodology.

The results describe the representative model and should not be interpreted as evidence of actual construction deficiencies or incomplete field inspections.
  