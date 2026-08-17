# Open Digital Specimen (openDS) — Overview & Compound Objects

This repository provides an overview of the **Open Digital Specimen (openDS)** data specification developed by [DiSSCo](https://www.dissco.eu/), including key architectural components of a Digital Specimen and guidance on modeling compound objects (such as multi-organism insect trays or mixed lots).

---

## Key Components of a Digital Specimen

A Digital Specimen (`ods:DigitalSpecimen`) acts as the machine-actionable FAIR Digital Object (FDO) surrogate for physical collection objects. Its core metadata and sub-structures include:

### 1. Persistent Identifiers & System Provenance
* **`@id` & `dcterms:identifier`**: Canonical DOI string (e.g., `https://doi.org/10.3535/XXX-XXX-XXX`).
* **`ods:physicalSpecimenID` & `ods:normalisedPhysicalSpecimenID`**: Resolvable physical catalogue number or compound ID bridging local Collection Management System (CMS) records with global indices.
* **`ods:organisationID` & `ods:sourceSystemID`**: ROR/Wikidata identifiers for the holding institution and Handle identifiers for the source data provider.

### 2. Categorization & Topic Taxonomies
* **`ods:topicOrigin`**: `Natural`, `Human-made`, `Mixed origin`.
* **`ods:topicDomain`**: `Life`, `Earth System`, `Environment`, `Extraterrestrial`, `Cultural Artefacts`, etc.
* **`ods:topicDiscipline`**: `Botany`, `Zoology`, `Palaeontology`, `Geology`, `Microbiology`, `Anthropology`, etc.
* **`ods:livingOrPreserved`**: `Living` vs. `Preserved`.

### 3. Sub-Entities & Nested Relationships
* **Specimen Parts ([`specimen-part.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/specimen-part.json))**:
  Handles multi-part specimens (e.g., skin + skull, herbarium sheet + microscopic slide, or multiple pinned organisms in a tray) that share a primary catalogue number.
* **Identifications ([`identification.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/identification.json) & [`taxon-identification.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/taxon-identification.json))**:
  Captures taxonomic determination history, verifying authority, type status (e.g., holotype, isotype), taxonomic rank hierarchy, HTML formatted labels, and accepted synonymy mappings.
* **Events & Locality ([`event.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/event.json), [`location.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/location.json), [`georeference.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/georeference.json))**:
  Models gathering/observation events, field notes, sampling protocols, life stages, verbatim and standardized administrative geography, coordinate bounding/WKT footprints, and georeference uncertainty metrics.
* **Geological & Chronological Context ([`geological-context.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/geological-context.json), [`chronometric-age.json`](openDS/data-model/fdo-type/digital-specimen/0.4.0/schema/chronometric-age.json))**:
  Captures litho-, bio-, and chrono-stratigraphy (Eon, Era, Period, Epoch, Formation, Member, Bed) alongside uncalibrated/calibrated radiometric assay determinations.
* **Assertions, Citations, & Entity Relationships**:
  Arrays for phenotypic/morphometric measurements (`ods:hasAssertions`), bibliographic references (`ods:hasCitations`), and typed semantic graph links (`ods:hasEntityRelationships`).

---

## Modeling Compound Objects in openDS

A Digital Specimen can represent a compound object comprising multiple physical entities (e.g., multiple insects in a single tray, mixed fossil slabs, or multi-organism lots). Depending on the curation and identification strategy, three patterns are available:

### 1. Sub-Entities via `ods:hasSpecimenParts` (Recommended for Trays/Lots sharing one Accession ID)
When an entire container/tray shares a single primary catalogue number without individual catalogue IDs for each organism, the parent `ods:DigitalSpecimen` holds the shared collection event and provenance, while each distinct organism or taxon is modeled as an `ods:SpecimenPart` under `ods:hasSpecimenParts`.

See the complete example in [**`insect_tray_digital_specimen.json`**](insect_tray_digital_specimen.json).

### 2. Homogeneous Multi-Organism Lots
If the compound unit contains multiple individuals of the same taxon under one record, top-level Darwin Core terms quantify the lot directly:
* `dwc:organismQuantity`: `"24"`
* `dwc:organismQuantityType`: `"individuals"`
* `dwc:organismScope`: `"tray"` / `"lot"`

### 3. Discrete Digital Specimens Linked via `ods:hasEntityRelationships`
If each individual pinned insect or specimen has its own unique unit ID/barcode, each is published as an independent `DigitalSpecimen` FDO and linked together or to the container via `ods:hasEntityRelationships` (e.g., `dwc:relationshipOfResource: "containedInTray"`).
