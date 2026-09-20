# Supplementary Data 3. LLM prompts and API parameters

> **Verification note for the authors:** the prompts below are reconstructed from the task
> descriptions in Section 2.2. Before submission, replace them with the verbatim prompts
> actually used, and confirm every parameter value against the preserved API logs.

## 1. Model and API configuration

| Item | Value |
|---|---|
| Model | DeepSeek-v4-flash (DeepSeek, Hangzhou, China) |
| Access | DeepSeek API, August 2026 |
| Temperature | 0.0 (all other sampling parameters at API defaults) |
| Batch strategy | Records processed in batches of 20 titles and abstracts |
| Output format | Structured output (JSON); model prohibited from generating, correcting or completing bibliographic metadata |
| Alternative model (repeat-coding check) | Kimi K3 (Moonshot AI), identical prompt and post-processing protocol |
| Seed-variation configuration | Non-zero decoding temperature with three random seeds (production setting used temperature 0.0) |
| Prompt variants | Three semantically equivalent variants (P1–P3) |

## 2. Entity-normalization prompt (draft for verification)

**System:** You are a bibliographic data curation assistant. You only propose candidate
mappings among name variants. You never invent, complete, or correct bibliographic records.

**Task:** Given the following batch of 20 titles and abstracts (indexed 1–20), identify
author, institution and country names that plausibly refer to the same entity
(full names, abbreviations, spelling variants). Return a JSON array of objects with fields:
`entity_type` (author | institution | country), `canonical_form`, `variants` (array),
`evidence` (record indices), `confidence` (high | medium | low). Propose mappings only
when supported by the supplied text.

## 3. Theme-coding prompt (draft for verification)

**System:** You are a semantic coding assistant for crop-science literature. You extract
semantic units and propose consolidations. You never add information not present in the
supplied titles and abstracts.

**Task:** For each record (index, title, abstract), extract semantic units in four facets:
research object, biological process, trait, and technology. Then propose merges among
synonymous or near-synonymous expressions, and flag candidate splits where one expression
mixes two distinct concepts. Return a JSON array with fields: `record_index`,
`semantic_units` (array of {facet, term}), `merge_proposals` (array of {from, to}),
`split_proposals` (array of {term, reason}).

## 4. Repeat-coding protocol

- P1: the production prompt above.
- P2/P3: semantically equivalent paraphrases of P1 with identical task constraints and output schema.
- Seed runs: three runs at non-zero temperature, seeds recorded per run.
- Alternative-model run: Kimi K3 (Moonshot AI) with the identical P1 prompt, batch size and post-processing rules.
