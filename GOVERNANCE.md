# How the PTPGE specification is governed

PTPGE is an open specification: a vocabulary of print concepts with permanent URIs, a deterministic identity
for a configured product (`ptpge-cfs/1`), and the JSON Schemas that use both. This document describes who
decides what goes into it, and how. It is short because the structure is young; it will grow with the
association that is being formed to hold the specification.

## Roles

**Maintainers** curate the vocabulary, review proposals, record decisions and publish data releases. Today the
maintainers are the founders (Patrik Knutsson, Torben Pedersen). **Contributors** are anyone who proposes,
corrects or challenges a term — printers, suppliers, software vendors, buyers, authors of agents.
**The association** (PTPGE, a non-profit, being formed) will hold the specification, appoint maintainers and
own this process; until it exists the maintainers act in its stead and say so.

## What is decided here, and what is not

The specification decides *meaning* and *identity*: which concepts exist, what they are called and how they
are defined in each language, where they sit in the hierarchy, how they map to source standards (XJDF, ISO,
GS1 …), and how a configuration is canonicalised and hashed. It does not decide prices, availability,
business terms, or how any implementation looks or behaves. Implementations — commercial or not — are
outside this process and have no privileged voice in it.

## How a proposal becomes a term

1. **Proposal.** Open an issue in the repository (or write to post@ptpge.org) with label, definition, parent
   term and any equivalents in other standards. See `CONTRIBUTING.md`.
2. **Review.** A maintainer checks the proposal against the term criteria (a concept, not a brand; one
   category per label; absence means «none») and asks back where needed. Discussion happens in the issue.
3. **Decision.** The maintainers decide: adopt, adopt with changes, defer, or decline — with the reason stated
   in the issue. Every adopted change receives a **decision record** number (`SD-NNNN`); the record names the
   date, the decision, the reasoning and the issue it came from.
4. **Release.** Changes are published in the next data release (`vYYYY-MM-DD`), listed in `CHANGELOG.md` with
   the record number, and tagged in this repository. Term URIs never break: a retired term stays resolvable,
   marked `deprecated`, pointing to its successor.

Corrections to existing terms — a wrong label, a mistranslation, a misplaced parent — follow the same path and
are normally fast.

## Decision records

Decisions about the vocabulary have been numbered since 2024. The records were kept as an internal working log
and are being separated from notes that do not belong in a public register. From the next data release on,
every vocabulary change gets a public decision record in `decisions/` in this repository; older records are
added as they are cleaned.

## Status of terms

Each term carries a `status`. `proposed` means it is in the vocabulary and usable but has not been through a
working-group review; `approved` means it has. Much of the vocabulary is `proposed` during the Public Beta —
that is what the beta is for.

## Contributions and licence

The specification is published under CC BY 4.0. Contributions are accepted under the same licence, inbound as
outbound: by proposing a term or a correction you agree that it may be published as part of the specification
under CC BY 4.0, with attribution to the specification; contributors are credited in the decision record.
Whether the association adopts a Developer Certificate of Origin or a Contributor License Agreement on top of
this is a decision for its first board.

## Conduct

Everyone taking part is expected to follow the `CODE_OF_CONDUCT.md` (Contributor Covenant 3.0).

## Changing this document

Changes to this document are proposed as pull requests and decided like any other change, with a decision
record.
