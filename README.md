# Human Provenance in Film

**HPF AI Disclosure Taxonomy -- v0.9, Draft for Consultation**

Consultation closes 30 June 2026.

---

## What This Is

A three-tier classification standard for AI disclosure in film and television, designed to travel in chain of title and deal documentation. No new technical systems are required.

Without a consistent disclosure standard, there is no reliable market data on whether buyers and audiences distinguish between AI-generated and human-authored content.

---

## The Three Tiers

Built on a single organizing principle: **does AI enhance human creative work, or does it replace human creative roles?**

| `hpf_classification` value | Label | Definition |
|---|---|---|
| `no_ai` | No AI Used | No AI tools were used at any stage of this production. All elements are human-authored. |
| `assistive_ai` | Assistive AI | AI enhanced or optimized elements created by human crew. No human creative or production roles were replaced. |
| `generative_ai` | Generative AI | AI synthesized or generated content that a human crew member would otherwise have created. Human roles were replaced. |

For the organizing principle, classification test, and schema reference: [taxonomy.md](taxonomy.md). For full definitions, scope, and edge cases: [HPF_Taxonomy_v0_9_draft_for_consultation.pdf](HPF_Taxonomy_v0_9_draft_for_consultation.pdf).

---

## How Classification Is Delivered

The HPF classification travels in chain of title documentation as a signed producer declaration. It is not derived from technical analysis of the content; the receiving party (distributor, platform, broadcaster) relies on the declaration.

Where technically implemented, the classification may also be present in or alongside the delivery file as a C2PA Content Credential. This is optional and complementary: the paper declaration in chain of title is the primary compliance mechanism. See [c2pa-mapping.md](c2pa-mapping.md) for the proposed technical implementation.

---

## Repository Contents

| File | Description |
|---|---|
| [taxonomy.md](taxonomy.md) | Tier reference, organizing principle, and classification test. For full definitions and edge cases see the PDF. |
| [HPF_Taxonomy_v0_9_draft_for_consultation.pdf](HPF_Taxonomy_v0_9_draft_for_consultation.pdf) | Full taxonomy specification: definitions, scope, edge cases, and implementation guidance. |
| [schema.json](schema.json) | JSON Schema for HPF metadata fields. For platform engineers and delivery system developers. |
| [c2pa-mapping.md](c2pa-mapping.md) | Proposed mapping of HPF tiers to C2PA assertion types. Working proposal. |
| [HPF_AI_Disclosure_Sample_Template.pdf](HPF_AI_Disclosure_Sample_Template.pdf) | Sample producer declaration form. |
| [GOVERNANCE.md](GOVERNANCE.md) | Amendment process, version history, and governance handoff commitment. |
| [LICENSE.md](LICENSE.md) | CC BY 4.0 and patent non-assertion. |

---

## License

CC BY 4.0. No patent rights are asserted over the taxonomy methodology. See [LICENSE.md](LICENSE.md).

---

## Contact

contact@humanprovenance.film | [humanprovenance.film](https://humanprovenance.film)
