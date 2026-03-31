# Human Provenance in Film

**HPF AI Disclosure Taxonomy -- v0.9, Draft for Consultation**

Published by The Mise En Scene Company (MSC).
Consultation closes 1 May 2026.

---

## What This Is

A three-tier taxonomy for disclosing AI use in film and television productions. It is designed to travel through existing chain of title and deal documentation. No new technical systems are required.

The taxonomy exists to make the commercial value of human authorship legible. Without a consistent disclosure standard, there is no reliable market data on whether buyers and audiences distinguish between AI-generated and human-authored content.

---

## The Three Tiers

Built on a single organising principle: **does AI enhance human creative work, or does it replace human creative roles?**

| Tier | Label | Definition |
|---|---|---|
| 01 | No AI Used | No AI tools were used at any stage of this production. All elements are human-authored. |
| 02 | Assistive AI | AI enhanced or optimised elements created by human crew. No human creative or production roles were replaced. |
| 03 | Generative AI | AI synthesised or generated content that a human crew member would otherwise have created. Human roles were replaced, wholly or in part. |

Full definitions, examples, edge cases, and implementation guidance are in [taxonomy.md](taxonomy.md).

---

## How It Works in Practice

1. The producer completes a signed HPF AI Disclosure Declaration (one-page form: select a tier, describe any AI use, sign).
2. The sales agency includes the signed declaration as a representation in the acquisition agreement.
3. The HPF classification travels forward in deal memos and materials packages to all buyers, distributors, and platforms.
4. Where there is no sales agent, the producer submits the declaration directly to the receiving party as part of deal documentation.

Verification standard: reasonable commercial reliance on the producer's written declaration. Not a technical audit.

---

## Repository Contents

| File | Audience |
|---|---|
| [taxonomy.md](taxonomy.md) | Everyone. Full tier definitions, classification test, edge cases, implementation. |
| [HPF_AI_Disclosure_Sample_Template.pdf](HPF_AI_Disclosure_Sample_Template.pdf) | Producers. Printable one-page declaration form. |
| [schema.json](schema.json) | Platform engineers and delivery system developers. JSON Schema for HPF metadata fields. |
| [c2pa-mapping.md](c2pa-mapping.md) | Technical implementers. How HPF tiers map to C2PA assertion types. |
| [GOVERNANCE.md](GOVERNANCE.md) | Amendment process and handoff commitment. |
| [LICENSE.md](LICENSE.md) | CC BY 4.0 and patent non-assertion. |

---

## Licence

CC BY 4.0. MSC asserts no patent rights over the taxonomy methodology. See [LICENSE.md](LICENSE.md).

---

## Contact

contact@humanprovenance.film -- [humanprovenance.film](https://humanprovenance.film)
