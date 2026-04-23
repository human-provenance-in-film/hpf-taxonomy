# HPF Integration Guide

**Version 0.9 -- Working Document**

This guide is for developers implementing HPF classification in products and pipelines. It covers four implementation contexts: platform and ingest systems (including AI detection tool integration), sales agency and distributor technical systems, C2PA and Content Credentials tooling, and delivery portal and post-production tools building toward adoption.

The HPF taxonomy and schema are described in [taxonomy.md](taxonomy.md) and [schema.json](schema.json). The C2PA mapping is in [c2pa-mapping.md](c2pa-mapping.md). This guide assumes familiarity with those documents.

---

## One Thing to Understand Before Implementing

**HPF classification is a declaration, not a detection.**

The `hpf_classification` value originates from a signed producer declaration held in the chain of title. It is not derived from analysing the content of the file. A platform or ingest system translating that declaration into the schema is carrying forward a human attestation, not generating a machine signal.

Two implications follow:

1. **Do not infer or validate `hpf_classification` from content analysis.** The value is what the producer declared. If a platform has independent AI detection capabilities, those are a separate signal and should not be used to override or backfill an HPF value.
2. **The paper declaration is the authoritative record.** A C2PA Content Credential carrying an `hpf.film.ai_disclosure` assertion is a technical complement to the chain of title declaration, not a replacement for it. If the two conflict, the signed producer declaration governs.

---

## Platform and Ingest System Developers

This section is for content management systems, content catalogue databases, player platforms, broadcaster ingest systems, and delivery portal developers at distributors and streamers.

This is the primary implementation context for HPF at v0.9. The translation of a signed producer declaration into a code declaration happens here: at the point where a delivery is received and catalogued.

### Translating the declaration

When a delivery arrives with an HPF declaration, read the declared classification and write it to the two schema fields:

```json
{
  "hpf_taxonomy_version": "0.9",
  "hpf_classification": "assistive_ai"
}
```

`hpf_classification` must be one of the three defined values: `no_ai`, `assistive_ai`, `generative_ai`. These map directly to the categories in the producer's declaration. If the declaration uses natural language rather than the enum values, return it to the producer or sales agent for clarification before writing a value. Do not interpret or assign a classification independently.

Store the full schema object alongside the title record in your content catalogue or CMS. Include both fields in metadata exchanges with distribution partners where AI disclosure is relevant, rather than the classification value alone. Do not pass `hpf_classification` without `hpf_taxonomy_version`. The version field is required: the classification test may change across major versions.

### Deliveries without an HPF declaration

Where a delivery arrives without an HPF declaration, the recommended behaviour is a soft failure: ingest proceeds, and no HPF schema record is written. Do not block ingest on the absence of an HPF declaration at v0.9; adoption is not yet widespread enough to make this a hard requirement without creating friction for legitimate deliveries. Flag the absence for follow-up with the producer or sales agent through your standard content operations workflow.

Do not write `no_ai` as a default for content that arrives without a declaration. `no_ai` is a positive assertion that requires an explicit producer declaration. An absent record and `no_ai` are not equivalent.

### Validation

Validate incoming `hpf_classification` values against the enum in [schema.json](schema.json). Values outside `no_ai`, `assistive_ai`, `generative_ai` are invalid and should be flagged for manual review rather than written to the content catalogue. Do not silently discard invalid values or substitute a default. Flag missing or malformed `hpf_taxonomy_version` for manual review in the same way.

An invalid value should be treated as a soft failure consistent with the approach for missing declarations: ingest proceeds, the HPF record is not written, and the issue is flagged for follow-up with the producer or sales agent.

### Absent values and historical content

Do not write an HPF schema record where no declaration exists. Both fields are required by the schema, so a partial record with null field values is invalid. In your content catalogue or CMS, represent the absence of a declaration as a missing HPF record at the title level, not as a schema object with empty fields.

For library content produced before AI tools were in practical use in film production, some platforms may wish to apply `no_ai` in bulk. This is reasonable only where the platform has sufficient knowledge of the production circumstances to make the declaration responsibly — for example, for content the platform produced itself, or where the original production company has confirmed the position. For third-party licensed content, the platform is not in a position to make this declaration on the producer's behalf. The appropriate state for unverified historical content is no HPF record.

Whether HPF should formally define a `pre_standard` or `undisclosed` enum value to distinguish unverified historical content from actively declared `no_ai` is an open question for the consultation. Input is welcome.

### Display

HPF does not currently prescribe how platforms should display classification to end users. Display standards are an open question for discussion before v1.0.

What HPF does specify: the classification must not be altered or reinterpreted in display. A title declared as `assistive_ai` must not be shown as `no_ai`.

One non-binding reference implementation: a label on the title detail page, using the category labels from [taxonomy.md](taxonomy.md), displayed alongside other production information (year, country, rating). For example:

- **No AI Used**
- **Assistive AI**
- **Generative AI**

Titles with no HPF record need not surface any label unless the platform chooses to indicate that no declaration is available. How to handle undisclosed content in display is at platform discretion.

Prominence, placement, and visual treatment are at platform discretion. Feedback on what display implementations are being considered is welcome via the consultation.

### What to expect at v1.0

v0.9 is a draft for consultation. The schema fields and enum values are stable and are not expected to change at v1.0. Changes at v1.0 are expected to affect the taxonomy definitions and edge case guidance, not the data model.

Records written with `"hpf_taxonomy_version": "0.9"` will remain valid after v1.0 is published. They do not need to be re-classified unless the classification of specific content changes as a result of revised category definitions — in which case the producer, not the platform, is responsible for issuing an updated declaration. Platforms should store the version field precisely as received to preserve the record of which taxonomy version governed the original classification.

### AI content detection tools

Platforms using AI content detection tools alongside HPF classification should treat the two signals as independent and complementary. HPF is a legal attestation about production provenance; a detection tool produces a technical signal about content characteristics. Neither substitutes for the other, and a detection signal should not be used to override, backfill, or contradict an HPF declaration.

Where a detection signal conflicts with a declared classification — for example, a detection tool flags apparent AI-generated content in a film declared `no_ai` — the correct response is to flag the discrepancy for human review and, if warranted, to raise it with the producer or sales agent. Do not automatically update the `hpf_classification` field on the basis of a detection result. The signed producer declaration governs; a detection result is a prompt for investigation, not a correction.

Developers of AI content detection tools integrating with HPF-aware platforms should surface both signals distinctly in their output, make the independence of the signals clear to their customers, and avoid presenting detection results as a classification in HPF terms.

---

## Sales Agency and Distributor Technical Staff

This section is for technical staff at sales agencies and distributors who manage chain of title documentation, acquisition CMS systems, and deal documentation workflows.

Read the Platform and Ingest System Developers section first. The guidance there on translating declarations, validation, and absent values applies equally here. This section covers the additional considerations specific to sales agencies and distributors.

Sales agencies and distributors occupy a critical position in the HPF chain: they are typically the first entity to receive a signed producer declaration, the entity that incorporates it into an acquisition agreement as a contractual representation, and the entity responsible for passing it downstream to multiple buyers simultaneously.

### Storing the declaration

A sales agency or distributor with an internal CMS should store the HPF classification at the title record level, using the same two-field schema as a platform:

```json
{
  "hpf_taxonomy_version": "0.9",
  "hpf_classification": "assistive_ai"
}
```

This creates a canonical record that can be passed to buyers without each buyer needing to re-translate the signed producer declaration independently. The declaration remains the authoritative record; the schema fields are how it is held and passed on in technical systems.

If the declaration uses natural language rather than the enum values, return it to the producer for clarification before writing a value. Do not interpret or classify independently.

### Multi-buyer scenarios

When the same title is sold to multiple buyers simultaneously — a streamer, a broadcaster, a theatrical distributor — each buyer receives their own copy of the signed producer declaration. The canonical schema record held by the sales agency is the source of truth. Each buyer's ingest system may create its own record independently; where possible, provide the schema fields directly in the metadata package alongside the declaration to reduce transcription errors at each buyer's ingest.

Where a buyer's system does not yet support HPF fields, provide the declaration and note the classification in the deal memo or delivery specifications. The classification should travel in some form with every deal regardless of whether the buyer's system can ingest it.

### Passing classification downstream

Include both `hpf_taxonomy_version` and `hpf_classification` in metadata packages sent to buyers, delivery portals, and festival submission systems. Do not pass the classification value alone; the version field is required for buyers to know which taxonomy governed the original declaration.

Where deal documentation incorporates the classification as a producer representation — which is the recommended approach — the contractual language should reference the HPF taxonomy version alongside the classification value, so that any future taxonomy revision does not retroactively alter the meaning of the representation.

---

## C2PA and Content Credentials Tooling

This section is for C2PA SDK implementers, CAI tooling developers, and anyone building systems that create, sign, or verify Content Credentials for film and television assets.

### The proposed custom assertion

HPF proposes a custom assertion with the label `hpf.film.ai_disclosure`. The payload matches [schema.json](schema.json), nested under a `data` key as part of the assertion structure:

```json
{
  "label": "hpf.film.ai_disclosure",
  "data": {
    "hpf_taxonomy_version": "0.9",
    "hpf_classification": "no_ai"
  }
}
```

`hpf_classification` must be one of the three values defined in [schema.json](schema.json). Treat schema.json as the authoritative source; do not rely on this document for the enum definition.

This is a working proposal. It has not been submitted to the C2PA working group. The relationship between this assertion and the existing `c2pa.actions` / IPTC `digitalSourceType` values is described in [c2pa-mapping.md](c2pa-mapping.md).

### Relationship to existing C2PA assertions

`hpf.film.ai_disclosure` is not a replacement for `c2pa.actions` assertions with IPTC `digitalSourceType` values. They operate at different levels:

- `c2pa.actions` records what a specific tool did to a specific file at a specific point in processing
- `hpf.film.ai_disclosure` records the production-level classification from a signed producer declaration

Both should be present where tooling supports it. A VFX tool that uses a generative AI model would record that via `c2pa.actions` with `trainedAlgorithmicMedia`. The `hpf.film.ai_disclosure` assertion on the finished film is a separate, higher-level statement derived from the producer's declaration across the whole production.

See [c2pa-mapping.md](c2pa-mapping.md) for the full mapping and its known limitations.

### Transcoding

Embedded C2PA manifests do not survive transcoding. Film assets are transcoded multiple times across the distribution chain.

The recommended approach is Durable Content Credentials: store the manifest externally, bound to the asset via a hard binding (cryptographic hash of the asset content) and one or more soft bindings (perceptual fingerprint or watermark) that allow the manifest to be rediscovered if separated from the asset. See the C2PA specification's [External Manifests](https://spec.c2pa.org/specifications/specifications/2.3/specs/C2PA_Specification.html#_external_manifests) section for implementation details.

The `hpf.film.ai_disclosure` assertion is particularly suited to the external manifest model. Because it originates from a signed producer declaration rather than from a processing tool, the receiving platform does not need to create the assertion at the moment of file processing. It can be attached to the manifest after the declaration is signed and the delivery is received.

### Trust and signing

C2PA signing requires an X.509 certificate issued by a CA on the C2PA trust list. Who should sign the `hpf.film.ai_disclosure` assertion in a film distribution context is an open question not resolved in v0.9. Candidates include the producer, the sales agent or distributor, and the receiving platform. Each carries different trust implications.

This question will be raised with the C2PA working group as part of the proposal process. If you are implementing C2PA signing for film assets and want to include `hpf.film.ai_disclosure`, contact contact@humanprovenance.film before doing so. Until the working group discussion concludes, coordinate with us before implementing rather than proceeding independently.

---

## Delivery Portals and Post-Production Tools

This section is for developers of delivery portal systems and post-production tools with delivery or export workflows.

**This section is forward-looking.** At v0.9, the translation of a signed producer declaration into the HPF schema fields happens at platform ingest, typically as a manual step by content operations staff. Delivery portals and post tools are not currently expected to perform this translation. This section describes what upstream implementation could look like as adoption grows.

At scale, manual translation of declarations into content catalogue fields is a data quality and operations problem, not just a technical one. Delivery portal adoption — where the classification is captured as a structured field at the point of delivery submission — is the path toward reliable, consistent data. Platforms experiencing data quality issues with manual ingest are encouraged to raise this with HPF via the consultation.

### Delivery portals

Delivery portals operate in two distinct contexts, each requiring different UX.

**Platform-operated portals** (intake interfaces used by content operations staff at the receiving distributor or streamer): the ops team translates the signed producer declaration into the classification field. Present the three-option selection alongside a free-text notes field where staff can record the source document reference. If no declaration arrives with the submission, leave the HPF record absent and flag the absence for follow-up — do not infer a value. Include the category definitions from [taxonomy.md](taxonomy.md) in-context so staff can verify the translation without leaving the portal.

**Producer-facing portals** (submission interfaces used by producers or sales agents to deliver content outbound): the producer fills the classification field themselves. Display the organizing principle and all three category definitions in-context at the point of classification, linking to [taxonomy.md](taxonomy.md) for the full classification test and edge cases. Do not require the producer to navigate to humanprovenance.film to classify. If the producer does not provide a declaration, the field should be optional at v0.9: submission proceeds, no HPF record is written.

Festival and broadcaster submission systems fall into this category. Producers submitting directly — without a sales agent — may be encountering HPF for the first time at the submission form. In this context, in-context definitions and a link to humanprovenance.film are especially important. Note that a declaration made at festival submission is not automatically incorporated into a contract; the festival or broadcaster receiving the submission should consider how to carry the classification forward into screening agreements and programme documentation if they wish to use it downstream.

In both cases, present the classification using the labels from [taxonomy.md](taxonomy.md):

| Display label | `hpf_classification` value |
|---|---|
| No AI Used | `no_ai` |
| Assistive AI | `assistive_ai` |
| Generative AI | `generative_ai` |

**Output:** write the result as a sidecar JSON file conforming to [schema.json](schema.json), or pass it directly to the downstream content catalogue system. If writing a sidecar file, use the naming convention `<asset_id>_hpf.json`, where `asset_id` matches the identifier used for the primary delivery asset in your system. A consistent naming convention is required for downstream ingest systems to locate the file without manual intervention. HPF does not yet prescribe a universal naming convention; coordinate with downstream partners before implementation.

**Audit trail:** record the timestamp, the source (producer declaration, ops staff entry, or producer self-declaration), and the identifier of the staff member or submitting party alongside the classification value. This audit trail has independent value for content operations and compliance, independent of downstream catalogue use. It is the basis for resolving disputes about what was declared and when.

**Amended declarations:** if a producer submits an updated declaration for a title already in the system, record the new classification as a versioned update rather than overwriting the original. Preserve the original declaration and timestamp. How amended declarations propagate to downstream catalogue systems and distribution partners is at the portal operator's discretion, but the original record should not be destroyed.

### Post-production tools

A post-production tool with a delivery or export workflow can present the HPF classification field to the operator at two points: in the project metadata panel as a persistent project-level field, and again as a confirmation step at export or delivery. Collecting it at the project level first, rather than only at export, means the classification is recorded before the delivery step and is less likely to be skipped or rushed.

**Who fills it in:** post-production tools are operated by technical staff — editors, colourists, finishing engineers, delivery coordinators — not typically by the producer who signed the declaration. Treat the tool operator the same way as content operations staff in a platform-operated portal: they are translating a signed producer declaration, not originating one. Present a free-text notes field alongside the classification dropdown where the operator can record the source document reference. The value must trace back to a producer declaration; the operator should not classify independently.

**Project-level scope:** HPF classification applies to the production as a whole. Map it to your tool's project or job entity — whichever is the top-level container for a single production — not to individual sequences, reels, assets, or output formats. If your tool handles multiple concurrent projects, each project carries its own classification field independently.

**Output and sidecar delivery:** write the classification as a sidecar JSON file conforming to [schema.json](schema.json), using the naming convention `<asset_id>_hpf.json` where `asset_id` matches the primary delivery asset identifier. Including a sidecar in a delivery package — IMF, DCP, or platform-specific bundle — requires prior agreement with the receiving party; many ingest systems will reject unexpected files. If no such agreement exists, record the classification internally and deliver it separately: as a standalone JSON file, via a delivery portal field, or alongside other paperwork. Do not embed the classification in the media file itself.

**In-tool AI feature usage:** post-production tools with built-in AI features — noise reduction, cleanup, AI-assisted subtitling, automated camera tracking — are in a position to know whether those features were used on a given project. This is distinct from inferring classification from content analysis, which is prohibited. Where a tool has used its own AI features on a project, it is reasonable to surface that usage to the operator as an input to the classification decision — for example, a summary of which AI features were active — while making clear that the final classification must come from a producer declaration, not from tool usage data alone. Do not pre-populate `hpf_classification` from tool usage. Do surface relevant tool usage information so the operator can have an informed conversation with the producer.

If you are building toward HPF support, contact contact@humanprovenance.film. Early coordination avoids inconsistent implementations.

---

## Open Questions

The following are unresolved in v0.9 and are part of the consultation (closes 31 October 2026):

- **Signing authority:** who holds and signs the `hpf.film.ai_disclosure` C2PA assertion in a distribution context
- **Display standards:** how platforms surface HPF classification to end users
- **Series and episodic content:** whether classification applies at the title, season, or episode level
- **Reclassification:** how to handle a change in declared classification between an original delivery and a subsequent cut or version
- **Undisclosed state:** whether a formal `pre_standard` or `undisclosed` enum value should be added to distinguish unverified historical content from actively declared `no_ai`
- **Amended declarations:** how updated producer declarations propagate through delivery portals and downstream content catalogue systems, and whether portals should version declaration records
- **Sidecar naming convention:** whether HPF should prescribe a standard filename pattern for sidecar JSON files to ensure interoperability across delivery systems
- **Music licensing provenance:** whether production music libraries should carry track-level AI provenance metadata (human-composed, AI-assisted, AI-generated) to support accurate production-level disclosure, and what form that metadata should take
- **Festival and broadcaster downstream use:** how a classification declared at submission should travel into screening agreements, programme documentation, and audience-facing display where no formal deal documentation exists

To contribute: open an issue or pull request on the repository, or email contact@humanprovenance.film.

---

## Version History

| Version | Date | Notes |
|---|---|---|
| 0.9 | April 2026 | Initial working document. |

---

contact@humanprovenance.film | [humanprovenance.film](https://humanprovenance.film)
