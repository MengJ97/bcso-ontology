# Bronze Casting Site Ontology (BCSO)

**BCSO：面向古代中国青铜铸造生产的双语本体** · v1.1

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Version: 1.1](https://img.shields.io/badge/version-1.1-blue.svg)]()
[![Persistent URI](https://img.shields.io/badge/URI-https://w3id.org/BCSO-orange.svg)](https://w3id.org/BCSO)

---

## 📖 Description

**Bronze Casting Site Ontology (BCSO)** is a bilingual (Chinese–English) domain ontology for systematically representing knowledge about **bronze casting production sites in ancient China**. Anchored to the international cultural-heritage reference ontology **CIDOC Conceptual Reference Model (CIDOC CRM; ISO 21127)**, BCSO captures the production chain of ancient Chinese bronze casting across six core modules:

| Module | Coverage |
|---|---|
| `CastingSite` | Production locales and their characteristics (workshop vs. integrated smelting–casting sites) |
| `AnthropogenicFeature` | Workshop facilities, activity areas (pits, kilns, furnaces, platforms, surfaces) |
| `ExcavatedObject` | Tools, raw materials, products, and production waste |
| `ProductionTechnology` | Smelting, mold-making, casting, heat-treatment, and surface-treatment techniques |
| `MetalMaterial` | Alloy recipes and raw materials (Cu-based, Sn, Pb, etc.) |
| `SpaceTimeActivity` | Period, administrative region, excavation activity, archaeological organization |

BCSO enables **interoperable, machine-readable documentation** of bronze-casting-site data and supports **SPARQL-based archaeological reasoning** across heterogeneous excavation reports and laboratory datasets.

**Ontology statistics** (v1.1, TBox only):

| Metric | Count |
|---|---|
| Domain-specific classes | 91 |
| CIDOC CRM classes (imported) | 9 |
| **Total classes** | **100** |
| Domain-specific object properties | 16 |
| CIDOC CRM object properties (imported) | 7 |
| **Total object properties** | **23** |
| Data properties (OWL 1 `owl:DatatypeProperty`) | 5 |
| Instance entities (Xiaomintun case study) | 137 |
| DL expressivity | ALCHIQ(D) |

> ⚠️ **Note**: The ontology header (`owl:versionInfo`) records `"1.0"` for compatibility with the in-review PeerJ manuscript (submitted 2026-10-03). The GitHub release tag is `v1.1` (released 2026-09-26) after renaming `ArchaeologicalUnit` → `ArcheologicalOrganization`. The content of the ontology file is unchanged between v1.0 and v1.1 except for that single class rename.

---

## 🗂️ Dataset Information

This repository contains the BCSO ontology schema and a real-world instance dataset from the Xiaomintun Shang Dynasty casting workshop.

| File | Format | Size | Description |
|---|---|---|---|
| `BCSO.ttl` | Turtle | ~47 KB | Core ontology schema (TBox). 100 classes, 23 object properties, 5 data properties. |
| `BCSO_Xiaomintun.ttl` | Turtle | ~117 KB | Instance data (ABox). 137 entities from the Xiaomintun casting workshop. Imports `BCSO.ttl`. |
| `bcso.owl` | OWL/XML | (in release v1.1) | Same ontology in OWL/XML format, distributed via [Release v1.1](https://github.com/MengJ97/bcso-ontology/releases/tag/v1.1). |
| `LICENSE` | Plain text | 11 KB | CC BY 4.0 license text. |

**External data sources** for the case study (excavation publications, not redistributed):

- Yue Z. *et al.* (2006/2007/2008), excavation reports of the Xiaomintun Shang bronze foundry site at Yinxu.
- Zhouyuan Archaeological Team (2011), excavation of the Zhouyuan bronze-casting remains.

---

## 🚀 Usage Instructions

### 1. Loading the ontology

**Option A — Protégé (recommended for browsing)**

1. Download [`BCSO.ttl`](https://raw.githubusercontent.com/MengJ97/bcso-ontology/main/BCSO.ttl) (or use `bcso.owl` from [Release v1.1](https://github.com/MengJ97/bcso-ontology/releases/tag/v1.1)).
2. Open in Protégé 5.6.5 (`File → Open...`).
3. Inspect the class hierarchy via the **OntoGraf** plugin.
4. Use the **ELK** or **HermiT** reasoner (`Reasoner → Start reasoner`) to classify the ontology.

**Option B — rdflib (Python, for SPARQL queries)**

```python
from rdflib import Graph

# Load the core ontology
g = Graph()
g.parse("BCSO.ttl", format="turtle")

# Load the instance data (imports BCSO.ttl)
g.parse("BCSO_Xiaomintun.ttl", format="turtle")

# Example SPARQL — count instances by class
q = """
SELECT ?class (COUNT(?inst) AS ?n) WHERE {
  ?inst a ?class .
  FILTER (STRSTARTS(STR(?class), "https://w3id.org/BCSO#"))
}
GROUP BY ?class
ORDER BY DESC(?n)
"""
for row in g.query(q):
    print(f"{row['class']}: {row['n']}")
```

### 2. Competency Question SPARQL queries

The eight competency questions (CQ1–CQ8) defined in the manuscript's Appendix B can be executed with any SPARQL-compliant tool. They are available as executable Python scripts distributed alongside the PeerJ submission.

### 3. Online browsing

The ontology resolves at the persistent URI <https://w3id.org/BCSO>, which redirects to this repository's interactive Protégé-generated documentation (HTML index in `contents.html`).

---

## 📦 Requirements

**To browse the ontology:**

- [Protégé 5.6.5+](https://protege.stanford.edu/) (free, open-source, cross-platform)
- Reasoner plugin: ELK or HermiT (bundled with Protégé)

**To run SPARQL queries / programmatic reasoning:**

| Requirement | Version |
|---|---|
| rdflib | 6.x+ |
| owlready2 | 0.4+ (optional, for OWL API access) |

**For DL expressivity and metric analysis:**

- OWL API 4.5.29 (Java) — required for `ALCHIQ(D)` analysis as reported in the manuscript.
- NEOntometrics (or a local owlready2 reproduction of OntoMetrics formulas) — for the structural metrics in Table VII of the manuscript.

---

## 🛠️ Methodology

BCSO was developed through an iterative ontology-engineering lifecycle following established methodologies (METHONTOLOGY; Noy & McGuinness, *Ontology Development 101*). The development followed a **"Term–Characteristics–Concept triple-driven framework"** instantiated in four phases:

1. **Specification & Conceptualization** — ISO terminology principles (ISO 1087, ISO 704) provided the methodological basis. Terminological standardization was derived from authoritative Chinese archaeological excavation reports and reference works (e.g., *Ancient Chinese Metal Technology*; *Encyclopedia of Chinese Archaeology*).

2. **Formalization** — Aristotelian ontology underpins the hierarchical structure through genus-differentia definitions. Object properties were designed to capture three fundamental dimensions: spatial hierarchy (`isLocatedIn`, `hasFeature`, `excavatedFrom`), temporal association (`belongsToPeriod`), and technological process (`involvesTechnique`).

3. **Implementation** — BCSO adheres to W3C semantic-web standards (OWL 2, RDF/XML, Turtle), formally grounded in Description Logics (DLs). The ontology uses `rdf:langString` for bilingual (Chinese–English) labels, supporting accessibility while maintaining compatibility with CIDOC CRM's `P3_has_note` pattern.

4. **Evaluation** — Validation combined (a) automated reasoning with ELK and HermiT, (b) structural-metric analysis using a local owlready2 reproduction of OntoMetrics formulas, and (c) competency-question evaluation (eight CQs) executed via SPARQL against the 137-entity Xiaomintun instance dataset.

**Alignment with CIDOC CRM** — BCSO is anchored to CIDOC CRM (ISO 21127) using a **"hierarchical correspondence and layered processing"** strategy. Core BCSO classes map to the most precise corresponding CRM entities (e.g., `CastingSite` → `E27_Site`; workshop components → `E25_Human-Made_Feature`); coarser CRM entities are mapped via hierarchical inheritance (BCSO classes subclass parent CRM classes while retaining domain-specific specializations). Object-property alignment is the primary mechanism for semantic correspondence. Mapping tables are provided in the manuscript (Tables V–VI).

**FAIR compliance** — BCSO follows the [FAIR principles](https://www.go-fair.org/fair-principles/):

- **Findable** — persistent URI at `https://w3id.org/BCSO`; Dublin Core metadata (`dcterms:creator`, `dcterms:description`, `dcterms:license`).
- **Accessible** — distributed in standard W3C formats (OWL 2, RDF/XML, Turtle); no proprietary formats or access restrictions.
- **Interoperable** — formal alignment with CIDOC CRM; uses rdfs:label for bilingual accessibility.
- **Reusable** — released under CC BY 4.0; version-tracked via `owl:versionInfo` and GitHub releases.

---

## 📚 Citations

If you use BCSO in academic work, please cite the associated peer-reviewed publication:

**Primary citation (PeerJ manuscript — under review):**

> Jiang M, Huang M, Chen K. (2026). *Bronze Casting Site Ontology: A bilingual ontology for modeling bronze casting production in ancient China*. **PeerJ Computer Science** (under review; manuscript ID 150181).

**APA:**

```
Jiang, M., Huang, M., & Chen, K. (2026). Bronze Casting Site Ontology:
A bilingual ontology for modeling bronze casting production in ancient
China. PeerJ Computer Science. (Manuscript ID 150181, under review)
```

**BibTeX:**

```bibtex
@article{bcso2026,
  author   = {Meng Jiang and Mingyu Huang and Kunlong Chen},
  title    = {Bronze Casting Site Ontology: A bilingual ontology for
              modeling bronze casting production in ancient China},
  journal  = {PeerJ Computer Science},
  year     = {2026},
  note     = {Manuscript ID 150181, under review},
  url      = {https://github.com/MengJ97/bcso-ontology}
}
```

**Related prior work** (BCSO complements this ontology for Shang-Dynasty finished artifacts):

> Jiang M, Lian H, Li C, Huang M, Chen K. (2026). *OSDBA: A bilingual ontology for the representation of Shang Dynasty bronze artifacts with archaeometallurgical knowledge*. **IEEE Access**, 14, 20659–20670. DOI: [10.1109/ACCESS.2026.3653659](https://doi.org/10.1109/ACCESS.2026.3653659).

---

## 📄 License & Contribution Guidelines

### License

This work is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.

You are free to:

- **Share** — copy and redistribute the material in any medium or format.
- **Adapt** — remix, transform, and build upon the material for any purpose, even commercially.

Under the following terms:

- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made. Citation should follow the format in the **Citations** section above.

See the [`LICENSE`](./LICENSE) file for the full legal text.

### Contribution Guidelines

Contributions are welcome via GitHub **Issues** and **Pull Requests**:

- **Bug reports / data issues** — please open an Issue describing the problem with reproducible steps.
- **Enhancement proposals** — please open an Issue first to discuss before submitting a PR.
- **Pull requests** — fork the repository, create a feature branch, and submit a PR with a clear description of the change.

### Maintainers

- **Meng Jiang** (蒋蒙) — Institute for Cultural Heritage and History of Science & Technology, University of Science and Technology Beijing.
- GitHub: <https://github.com/MengJ97>

---

## 📂 Repository Structure

```
bcso-ontology/
├── README.md                              # This file
├── LICENSE                                # CC BY 4.0 license text
├── BCSO.ttl                               # Core ontology (TBox)
├── BCSO_Xiaomintun.ttl                    # Instance data (ABox, 137 entities)
├── bcso.owl                               # OWL/XML serialization (Release v1.1)
├── contents.html                          # Protégé-generated HTML index
├── annotationproperties/                  # Protégé-generated HTML docs
├── classes/
├── dataproperties/
├── datatypes/
├── css/
├── images/
└── catalog-v001.xml                       # Protégé import catalog
```

---

*Last updated: 2026-10-07*
