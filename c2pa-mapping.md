# HPF Taxonomy -- C2PA Mapping

**Version 0.9 -- Working Proposal**

> This mapping is aspirational. HPF has not yet engaged the C2PA working group. The intention is to open a discussion at [c2pa-org/c2pa-spec](https://github.com/c2pa-org/c2pa-spec). A link to that discussion will be added here when it opens.

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

## Trust and Signing: Open Design Questions

HPF v0.9 does not prescribe a signing entity. The value of a Content Credential depends on the trustworthiness of the party that signs it. The signing entity must be independent of the producer and capable of anchoring to a recognized certificate authority.

The following candidates have been identified. The intention is to work through this with the C2PA working group and industry partners.

| Signing Entity | Pros | Cons | Feasibility |
|---|---|---|---|
| Industry body or trade association | High credibility; independent; natural home for a standards function | Slow to move; governance overhead; unlikely to take on operational liability without significant mandate | Low |
| Completion bond company | Already in chain of title; already performing independent verification; signature carries genuine commercial weight | Small number of firms; each would need to build signing infrastructure independently; competitive sensitivity | Medium |
| E&O insurer | Already underwriting the representation; signing is a natural extension of the attestation they already make | Conservative institutions; would need actuarial justification; slow procurement cycles | Low-Medium |
| Dedicated registry (purpose-built or EIDR-style) | Technically cleanest; neutral; scalable | Requires new infrastructure or a willing host organization; funding and governance unresolved | Medium |
| Distributor or platform at delivery | Signing party has strongest enforcement interest; no new intermediary required; fits existing delivery workflows | Credential created late in the chain; fragmentation risk if each platform implements differently | Medium-High |

Completion bond companies and distributors/platforms are the most viable near-term candidates. These are not mutually exclusive: a platform could accept a credential signed by a bond company in preference to issuing its own.

A dedicated registry becomes relevant if HPF reaches the point where a standards body or technology partner chooses to operationalize the signing function.

---

contact@humanprovenance.film
