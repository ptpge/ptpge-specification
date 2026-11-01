# PTPGE — the open specification of print

**PTPGE** (Periodic Table of Print & Graphic Elements) is the open specification of print: a vocabulary of terms
with permanent URIs, product templates that say which choices apply to a product and which are required, and
an identity — a complete configuration resolves to one fingerprint (`ptpge-cfs/1`) and one mark.

This repository is the **published data**, exactly as served by the API at `https://ptpge.org/v1/`. It is
regenerated from the maintainers' build on every release and tagged with the data version. Clone it, diff
releases, open issues against terms. Licence: **CC BY 4.0**.

- Specification site: https://ptpge.org
- Developers: https://ptpge.org/developers/ · API: https://ptpge.org/v1/ · OpenAPI: `v1/openapi.json`
- MCP server for AI agents: https://mcp.ptpge.org/mcp — source: https://github.com/ptpge/ptpge-mcp-server
- Changelog: `CHANGELOG.md` (also https://ptpge.org/changelog/)
- Current data version: see `v1/version.json`

## Layout

```
v1/
  index.jsonld            discovery document (what the API offers)
  scheme.jsonld           the SKOS concept scheme
  context.jsonld          JSON-LD context
  terms.jsonld, terms.ttl full bundle of every published term
  printing/  ordering/    the two sides of the vocabulary — one file per term (.jsonld + .ttl)
  variables/              the configurable axes (format, colour mode, fold, …) that bridge the two
  schemas/                document schemas: request/1, quote/1, vendor-declaration/1, supplier-catalogue/1, ucp-manifest/1
  suppliers/  vendors/    the thin registers (see schemas/vendor-declaration and supplier-catalogue)
  mappings/               crosswalks to other vocabularies
  openapi.json            the read-only API, OpenAPI 3
  deprecation-policy.json how terms are retired (URIs never break)
  version.json            data version, counts, languages
```

Every term file is JSON-LD 1.1 with SKOS relationships (`broader`, `narrower`, `exactMatch` …), a definition,
labels in the specification's languages, and a `status`. Deprecated terms stay resolvable and point to their
successor where one exists — see `v1/deprecation-policy.json`.

## Using it

Resolve any term by its URI — `https://ptpge.org/v1/printing/completions/folding/roll-fold` — with
`Accept: application/ld+json` or `text/turtle`, or read the same file from this repository under `v1/`.
The identity of a configuration (`ptpge-cfs/1`) is computed from term URIs, so systems that agree on the
vocabulary agree on the fingerprint. See https://ptpge.org/developers/ for the integration guide.

## Contributing

Proposals for new or changed terms are welcome as GitHub issues — see `CONTRIBUTING.md`. Terms are adopted
through the PTPGE decision process and published in the next data release; this repository is not edited
directly (pull requests against `v1/` will be closed, since the files are regenerated).

## Licence and attribution

Creative Commons Attribution 4.0 International (CC BY 4.0) — see `LICENSE`. Attribute as
«PTPGE — the open specification of print, https://ptpge.org, CC BY 4.0». PTPGE is held by PTPGE, a
non-profit association (being formed).
