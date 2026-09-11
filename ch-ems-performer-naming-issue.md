# Confusing naming: `Procedure.performer` vs. extension `ch-ems-ext-performer`

**Profile:** [CH EMS Procedure: Pretreatment](https://fhir.ch/ig/ch-ems/2.0.0-ballot/StructureDefinition-ch-ems-procedure-pretreatment.html) (v2.0.0-ballot)

## Observation

The StructureDefinition `ch-ems-procedure-pretreatment` defines a native `Procedure.performer` element alongside a sliced extension `ch-ems-ext-performer` (URL: `http://fhir.ch/ig/ch-ems/StructureDefinition/ch-ems-ext-performer`). Both carry the name "performer", but represent fundamentally different concepts:

| | `Procedure.performer` | `ch-ems-ext-performer` |
|---|---|---|
| **Type** | BackboneElement with `actor` (Reference to Practitioner, Organization, …) | CodeableConcept (code from IVR Pretreatment ValueSet) |
| **Semantics** | Reference to a concrete FHIR resource | Role/function as a coded value |

## Why it matters

For readers and implementers, the identical naming suggests a data duplication or redundancy where none exists. In the EMS context, the distinction is important:

- `Procedure.performer.actor` is **mandatory (1..1)** in FHIR R4 and requires a reference to a concrete resource — which is often unavailable in emergency situations.
- `ch-ems-ext-performer` allows capturing the **role/function as a code** without needing a reference — which is the actual use case for pretreatment documentation.

The design choice to use an extension is correct and well-motivated. However, the naming makes it unnecessarily difficult to understand the distinction.

## Suggested improvement

To reduce confusion, consider one of the following:

1. **Rename the extension** to a less ambiguous name, e.g.:
   - `ch-ems-ext-performed-by-code`
   - `ch-ems-ext-performer-role-code`
   - `ch-ems-ext-preparation-performer`

2. **Add a clarifying note** in the profile description explicitly stating that `ch-ems-ext-performer` is **not** a duplicate of `Procedure.performer`, but represents a coded role/function for the performer — distinct from the resource reference in the base element.
