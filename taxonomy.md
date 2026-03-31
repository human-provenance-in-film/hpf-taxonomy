# HPF AI Disclosure Taxonomy

**Version 0.9 -- Draft for Consultation**
Consultation closes 30 June 2026.

Full specification: [HPF_Taxonomy_v0_9_draft_for_consultation.pdf](HPF_Taxonomy_v0_9_draft_for_consultation.pdf)

---

## Organizing Principle

> Does AI enhance human creative work, or does it replace human creative roles?

---

## Tier Reference

| Value | Label | Definition |
|---|---|---|
| `no_ai` | No AI Used | No AI tools were used at any stage of development, production, or post-production. All elements are human-authored. |
| `assistive_ai` | Assistive AI | AI enhanced or optimized elements created by human crew. No human creative or production roles were replaced. |
| `generative_ai` | Generative AI | AI synthesized or generated content that a human crew member would otherwise have created. Human roles were replaced. |

These values correspond directly to `hpf_classification` in [schema.json](schema.json).

---

## Classification Test

For each use of AI in the production:

1. Would a human crew member have performed this function if the AI tool were not available?
2. If yes: did AI **enhance** their output, or did AI **replace** their contribution?
3. Enhance: `assistive_ai`. Replace: `generative_ai`.

A production is classified at the highest tier applicable to any element. A film with both AI noise reduction (`assistive_ai`) and an AI-generated score (`generative_ai`) is classified as `generative_ai`, with both uses described in the declaration.

---

## Consultation

v0.9 is a draft for consultation. Feedback can be submitted to contact@humanprovenance.film or via the GitHub repository (issues or pull requests). Responses received before 30 June 2026 will inform the v1.0 revision.

---

## License

CC BY 4.0. See [LICENSE.md](LICENSE.md).

contact@humanprovenance.film
