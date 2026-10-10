# Contributing to the PTPGE specification

PTPGE is built openly. Anyone in the printing industry — printers, suppliers, software vendors, buyers,
agents' authors — is welcome to propose, refine and challenge terms.

## How to propose a term or a change

Open a GitHub issue in this repository (or write to post@ptpge.org). Please include:

- **Label** in English (other languages welcome).
- **Definition** in plain language, written so a non-expert can understand it.
- **Where it belongs**: the parent term's URI (e.g. `https://ptpge.org/v1/printing/completions/folding`) or a
  clear description if you do not know the exact parent.
- **Optional**: synonyms / alternative labels; equivalents in ISO, FOGRA, GS1, Schema.org, eCl@ss, XJDF or
  Wikidata (`exactMatch` / `closeMatch`); references.

You do not need to assign a notation — maintainers do that on adoption.

## What is and is not a term

- A term names a **concept** of print production or print buying — a fold, a substrate class, a colour mode,
  a binding method. Brands, company names and product models are **not** terms; they belong in the registers
  (suppliers, vendors) or in catalogues outside the vocabulary.
- Alternative labels never cross category boundaries (a number is not a colour; a system is not a colour class).
- Absence means «none»: a configuration that does not ask for a finish is an unfinished product. Do not propose
  explicit «none» values.

## Process

Proposals are reviewed by the maintainers, decided through the PTPGE decision process, and published in the
next data release. Every release is tagged here (`vYYYY-MM-DD`) and described in `CHANGELOG.md`. Term URIs never
break: a retired term stays resolvable, marked `deprecated`, with a pointer to its successor where one exists.

## Pull requests

The files under `v1/` are generated from the maintainers' build and regenerated on every release, so pull
requests that edit them directly cannot be merged. Pull requests against `README.md`, `CONTRIBUTING.md` and
the documentation are welcome.
