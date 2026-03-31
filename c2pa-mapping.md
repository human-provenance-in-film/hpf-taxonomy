# HPF Taxonomy -- C2PA Mapping

**Version 0.9 -- Working Proposal**

> This is aspirational. MSC has not yet engaged the C2PA working group. We intend to open a discussion at [c2pa-org/c2pa-spec](https://github.com/c2pa-org/c2pa-spec) and will update this file when that happens.

---

## Proposed Assertion

**Label:** `hpf.film.ai_disclosure`

**Payload:** two fields, matching [schema.json](schema.json).

```json
{
  "hpf_taxonomy_version": "0.9",
  "hpf_classification": "no_ai"
}
```

`hpf_classification` is one of: `no_ai`, `assistive_ai`, `generative_ai`.

---

## Implementation Note

Film files are transcoded multiple times across the distribution chain. Embedded C2PA manifests do not survive transcoding. Durable Content Credentials -- manifests stored separately and associated with the file by content hash -- are the recommended implementation path.

The value of `hpf_classification` is derived from a signed paper declaration in the chain of title, not from technical detection of the content.

---

*Human Provenance in Film -- An initiative of The Mise En Scene Company*
*contact@humanprovenance.film*
