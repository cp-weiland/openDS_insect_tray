## Open Digital Specimen (openDS) — Compound Objects

This repository investigates and details architectural components of the **Open Digital Specimen ([openDS](https://github.com/DiSSCo/openDS))** specification developed by [DiSSCo](https://www.dissco.eu/) to model compound objects, such as multi-organism insect trays or mixed lots.

The Digital Specimen is DiSSCo's core data model. It expands on domain-specific aspects of the Extended Specimen concept while implementing the FAIR Digital Object data model, providing a FAIR Digital Object (FDO) type to represent physical collection objects, their derivatives (e.g., sequences, image annotations), core metadata policies, and sub-structures. Digital Specimens are implemented using openDS, which defines the relevant classes and properties within the ods: namespace.

---

## Modeling Compound Objects in openDS

A Digital Specimen can represent a compound object comprising multiple physical entities (e.g., multiple insects in a single tray, mixed fossil slabs, or multi-organism lots). Depending on the curation and identification strategy, three architectural patterns are available:

### 1. Sub-Entities via `ods:hasSpecimenParts`
**Best for:** Trays or lots sharing a single Accession / Catalogue ID.

When an entire container or tray shares a single primary catalogue number without individual catalogue IDs for each organism, the parent `ods:DigitalSpecimen` holds the shared collection event and provenance, while each distinct organism or taxon is modeled as an `ods:SpecimenPart` under `ods:hasSpecimenParts`.

* See the complete example in [**`insect_tray_digital_specimen.json`**](insect_tray_digital_specimen.json).

---

### 2. Homogeneous Multi-Organism Lots
**Best for:** A single record containing multiple individuals of the same taxon.

If the compound unit contains multiple individuals of the same taxon under one record, top-level Darwin Core (`dwc:`) terms quantify the lot directly:

```json
{
  "dwc:organismQuantity": "24",
  "dwc:organismQuantityType": "individuals",
  "dwc:organismScope": "tray"
}
```
3. Discrete Digital Specimens Linked via ods:hasEntityRelationships

Best for: Objects where each specimen or pinned insect possesses its own unique barcode / ID.

If each individual specimen has its own unique unit ID or barcode, each is published as an independent DigitalSpecimen FDO. They are linked together or to the parent container using ods:hasEntityRelationships:
```json
{
  "dwc:relationshipOfResource": "containedInTray",
  "dwc:relatedResourceID": "[https://doi.org/10.xxxx/container-id](https://doi.org/10.xxxx/container-id)"
}
```

