# PTPGE specification — changelog

What changed in each data release, newest first. Mirrors https://ptpge.org/changelog/.
Data releases are versioned by date; the API contract is `v1`.

## 2026-10-10

### Structure: Thin registers owned by ptpge.org; documents live on the business's own domain; quote/1; ptpge-id/1 (SD-0101)

**The registers are thin.** A Suppliers or Vendors entry now carries identity only — URI, name, country, domain, claim status, the `location` of the business's own document, the schema it `carries`, last validation and an optional `logo` read from that document — no notation, level, definition or hierarchy. The 37 unclaimed name-only entries are removed (14 suppliers and 1 vendor remain). The document is owned by the business and lives at a well-known path on its domain: `/.well-known/ptpge/declaration.json` (vendor-declaration/1) or `/.well-known/ptpge/catalogue.json` (supplier-catalogue/1); ptpge.org validates it on claim and weekly, and never stores a copy. Admission is mechanical (control of the domain + a document that validates). **New schema `quote/1`**: a printer's answer to one request — same fingerprint, quantity, price, delivery, validity — addressed to the buyer; it has a recipient, so no well-known path. **Identity mark:** `ptpge-id/1` = sha256("ptpge-id/1:" + identity URI), drawn with the PTPGE mark; the same for every implementation. The pilot vendor is renamed `vendors/trykkverk-norge` (301 from the old URI). Also in this release: the Geography definition no longer names any product or marketplace. **vendor-declaration/1 has a published JSON Schema** (`/v1/schemas/vendor-declaration/1.schema.json`, draft 2020-12): every vocabulary value is a term URI, never a label; a press is a supplier URI plus the maker's model designation; an offering is a product template plus, per variable, the values accepted; `generatedBy` is open for an implementation's own fields. A reference validator (shape → identity → vocabulary) ships with the specification tools; the other four schemas follow. **Definitions no longer speak of layers or name an implementation**: 40 terms (delivery methods, tools and guides, the Variables group, Brochures, rollups, foil, carbon methods, two retired maker nodes) said «Layer 1/2/3/4» or «PTPGE Configurator» where they now say «the vocabulary» and «an implementation's rules» — in every language. No URIs, labels or relationships change.

- `https://ptpge.org/v1/suppliers`
- `https://ptpge.org/v1/vendors`
- `https://ptpge.org/v1/schemas/quote/1`
- `https://ptpge.org/v1/schemas/vendor-declaration/1`
- `https://ptpge.org/v1/schemas/supplier-catalogue/1`
- `https://ptpge.org/v1/ordering/geography`

## 2026-10-07

### Rule: Absence means «none»: a finish you do not ask for is not applied (SD-0098)

**What is not specified is not applied.** A configuration without a surface-finish binding is an *unlaminated* product — complete and fingerprintable — whenever the product's envelope does not list that axis in `requiredVariables`. The required axes (setup model, format, orientation, colour mode) remain the ones a request must answer. Consequently the explicit value `printing/features/surface-finish/none` (proposed 2026-09-22) is *deprecated without successor* and removed from the Business Cards envelope, its only use; its alt-labels (unlaminated, ulaminert, unlackiert …) stay as synonyms for «no surface finish». No registered fingerprint changes. The mapper on ptpge.org/mapping no longer flags optional axes as missing, and the Playground agent does not ask about finishes the buyer did not mention.

- `https://ptpge.org/v1/printing/features/surface-finish/none`
- `https://ptpge.org/v1/ordering/products/business-cards`

## 2026-10-06

### Structure: Company names leave the vocabulary: Suppliers and Vendors registers, Schemas branch, VAR notation (SD-0097)

**The vocabulary now carries no company names.** The 53 maker nodes that lived under `printing/technologies/printing-finishing-equipment/…` and `…/paper/paper-suppliers/` (Heidelberg, HP, Komori, Lessebo, Sappi, … including the sub-brands Speedmaster and Indigo) are *deprecated*, each with `replacedBy` pointing to its entry in the new **Suppliers register** at `/v1/suppliers/<slug>` (type `ptpge:Supplier`, notation `SUP:…`, 49 entries — HP and Heidelberg merge their press and wide-format/finishing nodes into one identity each). The old URIs keep resolving. A maker's series, grades and models are not terms: they belong in the maker's own *supplier catalogue* (schema in preparation). Two more branches join `/v1/`: the **Vendors register** (`/v1/vendors/`, type `ptpge:Vendor`, `VEN:…`; first entry: the pilot printer Trykkverk) and **Schemas** (`/v1/schemas/<name>/1`, type `ptpge:Schema`, `SCH:…`; vendor declaration, supplier catalogue, request, UCP manifest — drafts). **What counts as a term** is now defined: a concept in Printing Process or Ordering Process that is not deprecated — `version.json` reports `terms` (728), `templates` (56), `variables` (35), `schemas`, `suppliers`, `vendors`, `deprecated` and `uris` separately. **Notation:** one letter means vocabulary (`P`, `O`); three letters mean everything else — Variables are renamed `V:…` → `VAR:…` (URIs unchanged). Term schema: `type` enum and notation pattern widened; context: `ptpge:Supplier/Vendor/Schema`, `makes`, `identityClaimed`, `catalogue`, `schemaVersion`, `stage`.

- `https://ptpge.org/v1/suppliers`
- `https://ptpge.org/v1/vendors`
- `https://ptpge.org/v1/schemas`
- `https://ptpge.org/v1/variables`
- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/printing-presses`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-suppliers`

## 2026-09-28

### Schema: Every variable carries the buyer's question (SD-0094)

**The Standard asks in the buyer's words.** Each of the 35 variable terms (`variables/…`) now carries `question` in twelve languages beside `label` and `definition`: «How is it put together?» for `setup-model`, «What size?» for `format`, «Printed in which colours?» for `color-mode`, «How is it bound?» for `binding`. One question per axis, phrased to hold for every value the axis can take — a roll and a box are «put together» as much as a tri-fold — so it applies to every product template without per-product wording. `label` stays the Standard's name for the axis; `question` is what a storefront or agent asks when walking a buyer through it. Context: `question` (`ptpge:question`, language container); schema: optional language map on variable terms. MCP `ptpge_get_term` returns it unchanged. The GutenLex Configurator (service 0.5.0) passes it through as `question` on each axis of `/axes`.

- `https://ptpge.org/v1/variables`
- `https://ptpge.org/v1/context.jsonld`

### Data: Orientation is required wherever a template supports it (SD-0093)

**One rule, no exceptions.** `variables/configuration/orientation` moves from *recommended* to *required* on all 50 product templates that carry `supportsOrientation`, right after format. A landscape A5 single fold is a different product from a portrait one — the fold runs the other way and the pages have another shape — and a file check must know whether the specification means 210 × 297 or 297 × 210. No identity is issued without it (SD-0083); square formats bind `orientation/square`. Identities computed before this change stay valid and keep resolving.

- `https://ptpge.org/v1/variables/configuration/orientation`
- `https://ptpge.org/v1/ordering/products`

## 2026-09-23

### Data: Paper Fibre as its own facet (recycled / fresh), PS6–PS8 paper classes, warm shades named — third release of the day

**Recycled is fibre origin, not a surface.** `paper-surfaces/recycled-paper` sat next to matte and silk, so nobody could say *recycled and silk* on one axis. New facet `paper-fibre` with `recycled-fibre` and `fresh-fibre`, a new axis `variables/substrate/substrate-fibre` and the envelope predicate `supportsSubstrateFibre` on the 16 product templates that offered recycled (flyers, postcards, envelopes, letterheads, newspapers …). The old value is *deprecated*, not deleted: it keeps its URI and page and points forward (`replacedBy`). Kept outside the headline configuration count. **PS6–PS8.** ISO 12647-2:2013 has eight printing-substrate classes; the Standard had five. Added PS6 super-calendered, PS7 improved uncoated and PS8 standard uncoated (wood-containing), 12 languages, offered on newspapers, flyers, brochures, magazines, catalogues and loose sheets (PS6 on magazines and catalogues). The coating-vs-class consistency rule (SD-0076) treats PS6–PS8 as uncoated. **Natural and Ivory** share CIE whiteness 80–90 and differ in warmth (b*); the labels now say so (*slightly warm* / *warm*) instead of repeating the same number. Supplier seed: Lessebo's series now bind fibre, class and surface where the data sheets support it (fresh fibre, PS5, matte), and the Supplier Profile builder shows Fibre and ISO class rows. 850 terms.

- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-fibre`
- `https://ptpge.org/v1/variables/substrate/substrate-fibre`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-surfaces/recycled-paper`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-classes`

### Data: Buyer words in Spanish, Japanese and Chinese on all 56 product templates (altLabels)

The findability matrix in the editor showed the gap: most product templates had no Spanish, Japanese or Simplified Chinese synonyms, so a buyer phrase in those languages depended on the embedding alone. **767 altLabels** added across the 56 templates — the words buyers actually type (*tarjeta de visita*, *tríptico*, *roll up*; 名刺, チラシ, のぼり旗; 名片, 易拉宝, 折页), trade and regional variants included. The SD-0071 collision sweep removed nine words that name a material or a process rather than the product (*vinilo*, *caja*, パッケージ, 折页 …). Kept as a reviewable data file (`tools/i18n/altlabels/`). Second release of the day.

- `https://ptpge.org/v1/ordering/products`

### Data: Portuguese (pt) and Hindi (hi) — eleventh and twelfth languages, full coverage on all 843 terms

Portuguese and Hindi join the Standard with complete coverage from day one: **843 labels and 843 definitions** in each, every scope note where the term has one, and conservative altLabels where a second trade name is established. Portuguese is one language (`pt`, Acordo-Ortográfico spelling readable in Brazil and Portugal, Brazil-leaning where the trade word differs — *gramatura*, *papel couché*, *offset plano*, *rotogravura*, *encadernação colada*, *vinco*, *cor especial*); Hindi is written in Devanagari and follows Indian trade practice — established English trade terms transliterated where that is what the trade says (ऑफसेट, लैमिनेशन), native Hindi where it is current (काग़, छपाई, रंग). Brand names, format codes and ISO references pass through unchanged. New in the toolchain: translations are kept as reviewable data files (`tools/i18n/`) applied by one idempotent script, so a professional review is a diff on JSON, not a rerun of scripts. Both languages are registered as `llm-draft` in `ptpge:translationStatus` on `/v1/scheme` with provenance and review channels (Brazil; North Indian print associations). Search index gains `has_def_pt` / `has_def_hi`; term pages, periodic table, dashboard, editor, configurator widget (0.3.23) and the demo storefront follow. No URIs, envelopes or fingerprints touched (SD-0077). SD-0089.

- `https://ptpge.org/v1/scheme`
- `https://api.ptpge.org/v1/search-index`

## 2026-09-22

### Data: Multi-page setup model: five faces, not six — «Outside Pages» removed, the text block is one face

The multi-page setup model declared six faces — front cover, inside front cover, *outside pages*, *inside pages*, inside back cover, back cover — while SD-0084 (surfaces follow the setup model) lists five, and nothing in the Standard said what distinguished the two interior faces. The outside/inside pair belongs to a folded sheet; a bound text block is simply pages. The configurator made the gap visible (Patrik: «hva menes med outside-pages og inside-pages?»). The interior is now one face, **Inside Pages** (nb *Innmat*, de *Innenteil*), with a description, and every face carries labels in ten languages. Nine templates use the model (booklets, books, calendars, catalogues, magazines, newspapers, notebooks, notepads, photo books). No offering, job or registered identity bound `outside-pages`; `ptpge_validate_config` now rejects the name, as it does any face the setup does not declare. Third release of the day (`dataVersion 2026-09-22`).

- `https://ptpge.org/v1/printing/configurations/setup-models/multi-page`

### Data: Common stock weights on the eight paper-weight bands (commonWeights)

Each paper-weight band now lists the stock weights a buyer actually meets inside it — `properties.commonWeights`: light-medium 115 · 120 · 130 · 135 · 140; medium 150 · 160 · 170; heavy-medium 180 · 190 · 200 · 210 · 220 · 230; heavy 240 – 300 in tens; extra heavy 310 – 440 in tens; extra light 50 · 60 · 70; light 80 · 90 · 100; extra thick 450 · 500 · 600 (Patrik, 22 September). The configurator shows them as tiles grouped by band, only for the bands a template allows; the substrate-weight variable stays numeric and the identity still binds the number. Second release of the day (`dataVersion 2026-09-22`, with the null values below).

- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-weights`
- `https://ptpge.org/v1/variables/substrate/substrate-weight`

### Data: «None» is a statement: a null value for Surface Finish and Embellishment, always in the envelope (SD-0088)

The configurator's one-question-at-a-time view asked «Which surface finish?» and offered gloss and matt lamination — with no way to say *no finish*. Skipping the question leaves the axis unspecified (the identity says nothing, the vendor's offer completes it); that is not the same as the buyer stating «no lamination, no varnish», which belongs in the identity. Colour already had its null value (*None (no color)*, an unprinted face); surface finish and embellishment did not. Two new values, both `standalone`: **None (no surface finish)** and **None (no embellishment)**, ten languages, definitions that spell out the difference from leaving the axis open. And a rule rather than 56 edits: for the three per-face feature axes the null value is **always in the envelope** — `ptpge_get_product_envelope` appends it to every `perSurfaceSupports*` list, `ptpge_validate_config` accepts it, and a vendor profile can never narrow it away (nothing has to be done to do nothing). Templates need not list it. No existing identity changes. `dataVersion 2026-09-22`.

- `https://ptpge.org/v1/printing/features/surface-finish/none`
- `https://ptpge.org/v1/printing/features/embellishments/none`
- `https://ptpge.org/v1/printing/features/colors/none`

## 2026-09-20

### Data: What may stand together: exclusive groups, standalone and multi-instance values on Color Mode and Surface Finish (SD-0084, cfs/1 R6)

The Standard said only «several allowed» for colour mode and surface finish, so «Black + CMYK + CMYKOGV + Spot» on one face was accepted and given an identity. Legal combinations depend on what the values *are*, not on how many there are — so three data fields carry the rule: `exclusiveGroups` on a variable (rule *at-most-one*), `standalone` and `multiInstance` on a value. First data: **Color Mode** group *process* = black, CMYK, CMYKOG, CMYKOGV; *none* stands alone; *spot colour* may repeat (Spot 1 + Spot 2 + Spot 3); white and spot are additive. **Surface Finish** group *lamination* = anti-scratch, gloss, matte, silk, soft-touch; varnishes are additive on top. Configuration Fingerprint v0.4 § 11 **R6**: two values of one group, a standalone value with company, or a repeated value that is not multi-instance → `exclusive_group_conflict`, no identity. No valid identity changes. Same day, second release: recommended axis *Fold / Binding Edge* and the four order variables (below). `dataVersion 2026-09-20`.

- `https://ptpge.org/v1/variables/features/color-mode`
- `https://ptpge.org/v1/variables/features/surface-finish`
- `https://ptpge.org/v1/printing/features/colors/spot-color`

### Data: Fold / Binding Edge is the recommended axis on 15 folded and bound templates (was «Folding»); the four order variables are listed

Two inconsistencies surfaced by the Layer 2 configurator, which shows the envelope exactly as the matrix states it. **(1)** Fifteen templates (brochures, booklets, books, catalogues, magazines, menus, cards, invitations, calendars, notebooks, notepads, NCR pads, tear-off pads, table cards, photo books) carry the envelope predicate `supportsFoldDirection`, whose values (left, top) belong to the variable `variables/finishing/edge` — but their `recommendedVariables` named `variables/finishing/folding`, a variable no template has an envelope for (its value domain repeats the setup models). Result: the real axis sat under «also configurable» and the recommended one never appeared. The recommendation now names **edge**; the variable's label becomes **Fold / Binding Edge** in ten languages (a bare «Edge» read as edge finishing); *Fold Direction* and *Binding Edge* stay as altLabels. **(2)** `variables/order` listed only `quantity` as narrower; `delivery-method`, `incoterm` and `lead-time` (created 26 June, broader → order) were unreachable through the hierarchy and so invisible to `ptpge_browse` and every configurator. All four are now listed — the commercial cluster of the `offer` profile (Configuration Fingerprint § 4) is complete. No URI, notation or definition changes; no fingerprint changes (recommended axes are advisory, SD-0083). `dataVersion 2026-09-20`.

- `https://ptpge.org/v1/variables/finishing/edge`
- `https://ptpge.org/v1/variables/order`
- `https://ptpge.org/v1/ordering/products/brochures`

## 2026-09-19

### Data: Buyer words in Swedish, Danish and Dutch on all 56 product templates (altLabels); Dutch «folder» moved to Brochures

The 56 product templates carried almost no buyer-facing synonyms in Swedish (10 words in total), Danish (1) and Dutch (14) against 403 in English — a buyer in those languages depended on the embedding alone. This release adds **264 Swedish, 253 Danish and 273 Dutch altLabels** (after removing 12 words that collide with non-product terms, e.g. *giclée*, *canvas*, *poster*) (3–8 per product: the words printers in those markets use on their own shops), following the one rule that keeps search deterministic: a word belongs to the product its definition names, never to a neighbour (*kupong* on Vouchers not Tickets; *mappe* on Folders not Binders; *broschyr/brochure* stays on Brochures, Booklets get *häftad broschyr / hæftet brochure*). One correction: the Dutch words *folder, folders, reclamefolders, strooifolders* sat on Flyers; a Dutch *folder* is a folded sheet, so they move to **Brochures** (Flyers keep *flyer, strooibiljet*). *drieluik* is deliberately not added to Brochures — it already names the tri-fold-stand setup model. Miss report: 30 new sv/da/nl buyer phrases in `tools/search_corpus.txt`, 101/101 resolve locally (6 misses before). Term vectors rebuilt. URIs, labels and notations unchanged; `dataVersion 2026-09-19`. Reviewed by Patrik (sv); da/nl open for correction by Danish and Flemish partners.

- `https://ptpge.org/v1/ordering/products/brochures`
- `https://ptpge.org/v1/ordering/products/flyers`
- `https://ptpge.org/v1/ordering/products/booklets`

## 2026-09-12

### Data: RISO joins Printing Presses; Harry Rochat definition corrected (British, not Swiss)

**RISO** (RISO Kagaku Corporation, Risograph digital duplicators and ComColor inkjet) is added as a maker identity under `printing/technologies/printing-finishing-equipment/printing-presses` — ten languages, status `proposed`; the Risograph model catalog (SF, MH, CV series) lives in the Layer 2 seed `oem-press-models.json`. The **Harry Rochat** definition described a Swiss manufacturer; Harry F. Rochat Ltd is British (Barnet, Hertfordshire) and builds etching, Albion, litho and bookbinding presses — definition rewritten in all ten languages from the maker’s own site. URI, label and notation unchanged. True count: **841 terms**.

- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/printing-presses/riso`
- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/printing-presses/harry-rochat`

## 2026-09-10

### Data: Two duplicate URIs removed: aluminium and wood had a stale leaf file next to their container

`board-materials/metal/aluminium.jsonld` and `board-materials/wood.jsonld` were leftovers from before the June 2026 board split turned both into containers (*solid* / *composite*). Two files, one URI each — the graph carried the id twice, the Vendor Profile builder showed the chip twice, and the published term count was inflated by two. The container files are canonical and unchanged; the leaves are gone. `build_api.py` now refuses to build a graph with duplicate ids. True count today: **840 terms** (838 + MGI + Scodix).

- `https://ptpge.org/v1/printing/substrates/board-materials/metal/aluminium`
- `https://ptpge.org/v1/printing/substrates/board-materials/wood`

### Data: MGI Digital Technology and Scodix — two digital-embellishment makers under Finishing Equipment

Two maker identities join `printing/technologies/printing-finishing-equipment/finishing-equipment`: **MGI Digital Technology** (JETvarnish 3D family) and **Scodix** (Ultra series). Maker identity is Layer 1; their model catalogs (formats, effects, substrates) live in the Layer 2 seed `oem-finishing-models.json` per the catalog-boundary doctrine (2026-08-23), where they can be picked as the machine behind a production line’s finishing. Ten languages, status `proposed`. No existing URIs touched.

- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/finishing-equipment/mgi`
- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/finishing-equipment/scodix`

## 2026-09-08

### Data: Polish (pl) — tenth language, full coverage on all 840 terms

Polish joins as the Standard's tenth language with complete coverage: **840 labels and 840 definitions** across every branch, following established Polish trade usage — *offset arkuszowy*, *offset zwojowy*, *wklęsłodruk*, *fleksografia*, *sitodruk*, *typografia*, *oprawa klejona*, *oprawa zeszytowa*, *bigowanie*, *wykrawanie*, *papier powlekany/niepowlekany*, *kolor dodatkowy*, *uszlachetnienie*, *podłoże*, *gramatura*. `altLabels.pl` follow the English altLabel set plus Polish trade synonyms; a cross-term collision sweep (SD-0071) pruned 15 entries before publication. Inch-based North American format labels keep their English form. Polish is registered as `llm-draft` in `ptpge:translationStatus` on `/v1/scheme`; professional review pending. No URIs, envelopes or fingerprints touched (SD-0077).

- `https://ptpge.org/v1/scheme`

## 2026-09-05

### Data: Winding direction: FINAT 1–8 as a bindable axis on roll products

Beta report #9 — kiss-cut stickers shipped on a roll — adds `variables/configuration/winding-direction` with the eight FINAT winding positions (1–4 printed face out, 5–8 face in; top, bottom, right or left edge leading), adopted from XJDF `Component/@WindingResult`, which references the same numbering. `supportsWindingDirection` on Labels and Stickers; Stickers now also reference the Roll setup model their definition always promised. A label applicator accepts one direction — buyer-visible, hence bindable (SD-0076 addendum). Nine languages, *proposed*. Validator: MCP 0.23.0.

- `https://ptpge.org/v1/variables/configuration/winding-direction`
- `https://ptpge.org/v1/printing/configurations/winding-direction`
- `https://ptpge.org/v1/ordering/products/stickers`

### Data: Event Tents: a 56th product template, with frame footprints as formats

Beta report #8 (a pop-up tent canopy sewn from four shaped pieces, plus a wall panel) adds **Event Tents** to the product templates and a format family of frame footprints (3 × 3, 3 × 4.5, 3 × 6 m; 10 × 10, 10 × 15, 10 × 20 ft). Format binds the frame; canopy and wall panels, seams and hems are production geometry derived by the producer (SD-0076 §5). The tent is the second independent case for the composite product model (canopy + walls as parts in roles) — design note updated, decision pending. Nine languages, *proposed*.

- `https://ptpge.org/v1/ordering/products/event-tents`
- `https://ptpge.org/v1/printing/configurations/formats/event-tents`

## 2026-09-04

### Data: Speedmaster joins the Standard as a sub-brand; press models stay in the Layer 2 catalog

Heidelberg’s **Speedmaster** is now an identity term under Heidelberg (`groupingType: vendor`), on the same test as HP Indigo: a sub-brand the trade names on its own (“printed on a Speedmaster”). The individual press — Speedmaster XL 106, 70 × 100 cm, up to 21,000 sheets/h — is deliberately *not* a term: models and SKUs are Layer 2 catalog content in the maker’s supplier profile and a printer’s vendor profile (SD-0039 addendum, 2026-09-04). HP Indigo gains the same `groupingType` for consistency.

- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/printing-presses/heidelberg/speedmaster`
- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/printing-presses/hp/indigo`

## 2026-09-03

### Data: Print condition and paper class become bindable; spine thickness and back orientation added (SD-0076)

Second beta-feedback release of the day, from production job tickets. **Print condition**: `supportsPrintStandard` is now populated on all 39 paper-based templates, and the seven **CRPC1–CRPC7** reference printing conditions (ISO/PAS 15339-2:2015 / CGATS 21-2) join the print standards. **Paper class**: new axis `variables/substrate/substrate-class` (ISO 12647-2 PS1–PS5) with `supportsSubstrateClass` on the same templates; validation rejects a coating that contradicts the class. **Spine thickness**: numeric mm axis with per-template ranges on books, photo books and notebooks. **Back orientation**: head-to-head / head-to-foot (XJDF LayoutIntent@Sides) on 14 two-sided templates. Fold terms now carry their XJDF FoldCatalog codes (F4-1, F6-1, F6-3, F6-4, F8-2, F8-4, F8-5) and nominal `panelProportions`; Format is defined as the finished (closed) format — and, for roll-up banners, as the *visible* graphic: the eight roll-up formats now publish a nominal per-edge `mountingAllowance` (cassette and leader-rail wrap) and no longer claim to be “produced to this fixed size”. Term pages show both panel proportions and mounting allowance under Cross-references as production geometry, not identity. All new terms are *proposed*, nine languages. The headline configuration count on the product-template preview rises from 279 million to **1.67 billion** (paper class counted as an optional single binding; print condition and back orientation bindable but deliberately outside the count — the figure is a floor, custom sizes make the space unbounded).

- `https://ptpge.org/v1/variables/substrate/substrate-class`
- `https://ptpge.org/v1/variables/configuration/spine-thickness`
- `https://ptpge.org/v1/variables/configuration/back-orientation`
- `https://ptpge.org/v1/printing/standards/print-standards/crpc6`

### Schema: Production geometry as data: panelProportions, mountingAllowance, and three new envelope fields

Two term fields carry *production geometry* that is deliberately not identity (SD-0076 §5): `panelProportions` on folded-sheet setup models (nominal flat-sheet panel widths) and `mountingAllowance` on hardware-mounted formats (nominal per-edge extra print the cassette consumes — a concept XJDF 2.2 does not carry). Three new product-template envelope fields: `supportsSubstrateClass`, `supportsBackOrientation` (URI lists) and `supportsSpineThickness` ({min, max, unit}); `supportsPrintStandard` is now populated. Context and JSON Schema updated; the API and MCP pass all fields through unchanged.

- `https://ptpge.org/v1/printing/configurations/formats/rollups/rollup-85x200`
- `https://ptpge.org/v1/printing/configurations/setup-models/folded-sheet/z-fold`
- `https://ptpge.org/v1/context.jsonld`

### Schema: XJDF 2.2 adopted as a source standard: the semanticSource field

New term field `semanticSource` (SD-0075). GutenLex is an aggregating standard — formats reference ISO 216, certifications reference FSC — and XJDF 2.2 now has the same status: where XJDF names a concept the Standard carries, the PTPGE term *is* that concept, normatively referenced on the term. Each entry carries the source standard and pinned version, the XJDF elements, the specification references and the match degree (exact / partial / concept). Derived mechanically from the published crosswalk (52 groups against XJDF 2.2) via `crosswalk-uri-map.json`; 466 terms carry the field in this release, rendered as *Semantic source* on term pages and passed through the API and MCP unchanged. Bibliographic, one-way and version-pinned: a source standard can never delay, block or change PTPGE. Serialization (ptpge-cfs/1), URIs, labels and existing fingerprints are untouched.

- `https://ptpge.org/v1/context.jsonld`
- `https://ptpge.org/v1/printing/features/colors/cmyk`
- `https://ptpge.org/v1/printing/configurations/setup-models/folded-sheet`

### Data: Beta feedback round: Fogra52, Books finishing facets, ISO 12647-2 paper classes

First Public Beta feedback release, from a tester mapping real xJDF-based job tickets against the Standard. Three additions: **Fogra52** (PSO Uncoated v3) joins Fogra39/51 under print standards; the **Books** template gains the finishing facets it lacked (surface finishes incl. five laminations, embellishments incl. foil stamping, plus the per-surface envelopes) so a varnished hardcover case is now describable; and a new **Paper Classes** sub-category carries the five printing-substrate classes of ISO 12647-2:2013 (PS1 premium coated … PS5 wood-free uncoated), each with `related` links to the ISO 12647-2 term, the characterizing Fogra datasets (PS1↔FOGRA51, PS5↔FOGRA52) and the coating terms. A **Ganging** term (gang-run / Sammeldruck) joins Workflow, giving the XJDF crosswalk row for SheetOptimizing a home. All new terms enter as *proposed* with nine languages.

- `https://ptpge.org/v1/printing/standards/print-standards/fogra52`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-classes`
- `https://ptpge.org/v1/ordering/products/books`

## 2026-08-25

### Data: Color layers: Color Mode becomes a multiple axis (White + CMYK et al.)

The `variables/features/color-mode` axis changes cardinality from single to **multiple**: a surface is configured with one or more color layers in print order — *White + CMYK* (white underprint on dark, clear or non-paper substrates), *CMYK + Spot*, *Black + White*. A single layer remains the common case, and existing configurations are unchanged. **White** was added to the color envelopes of the eight templates that print on rigid, textile or clear substrates but lacked it (canvas prints, expo displays, figures, flags, magnets, rollups, vinyl graphics, wine boxes), and the per-surface color envelopes were synced with the product-level lists. Validation accepts ordered multi-value bindings as-is.

- `https://ptpge.org/v1/variables/features/color-mode`
- `https://ptpge.org/v1/printing/features/colors/white`

## 2026-08-23

### Data: Catalog boundary, round two: supplier paper series leave Layer 1

URI change (Public Beta window): the 16 supplier product-series terms under `paper-suppliers/` (the Lessebo Colors/Design/Scandia 2000 family incl. shade leaves, Keaykolor, Olin Colors) moved to a Layer-2 catalog seed on the commercial side, with shade/coating metadata preserved. The five supplier *identities* — Lessebo, Antalis, Hahnemühle, Sappi, Holmen — remain in the Standard. The doctrine now reads generally: *maker and supplier identities belong to Layer 1 — presses, finishing equipment, wide-format printers, and consumable suppliers of any kind (paper, board, plates, ink); their product catalogs are self-declared Layer 2 content.* Finishing-equipment OEMs were already identities-only; future board, plate and ink suppliers enter as identities directly. 797 terms.

- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-suppliers`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-suppliers/lessebo`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-suppliers/antalis`

### Data: Catalog boundary enforced: press models leave Layer 1

URI change (Public Beta window): first enforcement of the catalog-boundary doctrine — *maker identities belong to the Standard; model catalogs belong to the actors*. The 18 model-level terms under `printing-presses/xerox/` (10) and `wide-format-printers/swissqprint/` (8) were removed from Layer 1; the Xerox and swissQprint *identities* remain, now as leaves like every other OEM. The models live on as a Layer-2 catalog seed on the commercial side (vendor builder unchanged for users) until self-declared OEM profiles exist — the same track planned for supplier paper series. 813 terms.

- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/printing-presses/xerox`
- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/wide-format-printers/swissqprint`

### Data: Cardboard as a rigid board — solid board and honeycomb board (field-test finding)

An agent field test (thanks, Torben) surfaced the gap: the trade's most sustainable display material had no home. New `board-materials/cardboard/` sub-category with two members — **Solid Board** (compressed fibre board, 1–3 mm, the standee and rigid-poster workhorse) and **Honeycomb Board** (paper honeycomb core, 10–16 mm, with Re-board, Falconboard and X-Board as trademark altLabels — the Dibond pattern). Figures and Signs support the whole cardboard family; Expo Displays gains honeycomb (and a thickness range, joining the SD-0070 templates). A rigid A0 poster on 3 mm board is now a valid, fingerprintable statement: signs + solid board + thickness 3 mm. The wider point-of-sale template round (floor displays, counter displays, shelf talkers) is queued as its own pass.

- `https://ptpge.org/v1/printing/substrates/board-materials/cardboard`
- `https://ptpge.org/v1/printing/substrates/board-materials/cardboard/solid-board`
- `https://ptpge.org/v1/printing/substrates/board-materials/cardboard/honeycomb-board`

## 2026-08-22

### Data: Substrate thickness — the missing numeric axis for rigid substrates (SD-0070)

Paper is specified by weight (gsm); boards are specified by **thickness** — and the Standard could not say «3 mm foam» until now. New numeric variable `variables/substrate/substrate-thickness` (canonical **mm**, decimals allowed) with envelope predicate `supportsSubstrateThickness` ({min, max} ranges) on the six templates with rigid or foil substrates: panel prints, signs, pavement signs, figures, magnets and wine boxes. US and film trade units convert exactly and live as altLabels — inches (3/16 in = 4.76 mm), caliper points (14 pt = 0.356 mm), mil (0.0254 mm), microns and film gauge. Buyer-optional per the SD-0069 doctrine: unbound means unspecified. «Papirtykkelse» moved off substrate-weight's altLabels — thickness is its own axis now.

- `https://ptpge.org/v1/variables/substrate/substrate-thickness`
- `https://ptpge.org/v1/ordering/products/panel-prints`
- `https://ptpge.org/v1/ordering/products/magnets`

### Data: Wall Art split into Panel Prints and Canvas Prints; ChromaLuxe moved to substrate

URI change (Public Beta window): the `wall-art` template was named after the *use*, not the object — the only one of 55. It also mixed two constructions: rigid panels and stretched textile. It is replaced by **Panel Prints** (`panel-prints` — acrylic, aluminium incl. dye-sublimation on coated metal, aluminium composite, wood, foam board) and **Canvas Prints** (`canvas-prints` — textile canvas on stretcher frame, gallery wrap). “Wall art” lives on as an altLabel on both, so store-category searches still resolve. **ChromaLuxe** moved from template altLabel to its correct shelf: a trademark altLabel on the `board-materials/metal/aluminium` substrate (the Dibond/Forex/Plexi pattern); `dye-sublimation` joined the panel template's technologies. Framing remains a completion, not a template — no “Framed X” product explosion. 827 terms; ~11.4 million describable configurations.

- `https://ptpge.org/v1/ordering/products/panel-prints`
- `https://ptpge.org/v1/ordering/products/canvas-prints`
- `https://ptpge.org/v1/printing/substrates/board-materials/metal/aluminium`

### Data: Substrate coating axis, paper shades populated, panel prints, technology cleanup

The substrates branch gained its missing primary distinction: **coating** (`paper-coatings/` — Coated / Uncoated, with variable `substrate-coating` and envelope predicate `supportsSubstrateCoating`; SD-0069). By doctrine it is a *buyer-optional refinement*: surface and shade remain the buyer-facing axes; unbound means unspecified. Coating parentheticals were removed from the five paper-surface labels (old wordings preserved as altLabels; URIs untouched). **Paper color shades** were populated across all 44 paper templates (White everywhere; full six-shade palette on the stationery/design products). **Panel printing**: Wall Art now spans canvas, acrylic, aluminium composite, wood and foam board — with trade names (Kapa, Forex, Plexi, Dibond, Multiplex) as altLabels. **Technologies normalized to family level** on all 54 templates (newspapers = coldset web offset, fine art = giclée). Magnets received its missing substrate: the new `magnetic-foil` term. 826 terms; ~11.4 million describable configurations.

- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-coatings`
- `https://ptpge.org/v1/variables/substrate/substrate-coating`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/magnetic-foil`
- `https://ptpge.org/v1/ordering/products/wall-art`

## 2026-08-21

### Data: Custom dimensions (SD-0066) + roll-up banner formats

Custom sizes are now first-class identities. Two new numeric variables, `variables/configuration/custom-width` and `custom-height` (mm, decimals allowed), are bound together if and only if Format = Customized. The canonical pair is normalized so **width ≤ height**; rotation of use is expressed via the existing Orientation variable — one physical object, one fingerprint. `Customized` availability was reviewed across all 54 product templates: 7 gained it (booklets, brochures, calendars, magazines, notebooks, notepads, posters), 3 lost it (binders, envelopes, rollups) — 49 of 54 now support custom sizing. Roll-ups instead gained a new formats sub-category with the industry's standard cassette sizes: the European family (85/100/120/150/200 × 200 cm) and the US family (23.5/33.5/47 × 80 in). No existing URIs or fingerprints touched.

- `https://ptpge.org/v1/variables/configuration/custom-width`
- `https://ptpge.org/v1/variables/configuration/custom-height`
- `https://ptpge.org/v1/printing/configurations/formats/rollups`
- `https://ptpge.org/v1/ordering/products/rollups`

## 2026-08-16

### Data: Dutch (nl) — ninth language, full coverage on all 810 terms

Dutch joins as the Standard's ninth language with complete coverage: **810 labels and 810 definitions** across every branch. 139 pre-existing Dutch labels — contributed through the ongoing field-mapping collaboration with VIGC (Flanders) — were preserved unchanged; the remaining labels and all definitions follow established Dutch/Flemish trade usage: *vellenoffset*, *rotatie-offset*, *diepdruk*, *garenloos binden*, *nieten (hechten)*, *rillen*, *stansen*, *gestreken/ongestreken papier*, *steunkleur*, *afwerking*. Conservative `altLabels.nl` on 21 terms (12 pre-existing preserved, 9 added: serigrafie, boekdruk, flexografie, rotogravure, tampografie, sublimatie, nieten/hechten …). Dutch is registered as `llm-draft` in `ptpge:translationStatus` on `/v1/scheme`; VIGC is the natural professional review channel. No URIs, envelopes or fingerprints touched.

- `https://ptpge.org/v1/scheme`

### Data: Danish (da) — eighth language, full coverage on all 810 terms

One day after Swedish, Danish joins as the Standard's eighth language with complete coverage: **810 labels and 810 definitions** across every branch. Terminology follows established Danish trade usage: *arkoffset*, *rotationsoffset*, *dybtryk*, *limbinding*, *klammerhæftning*, *rilning*, *udstansning*, *bestrøget/ubestrøget papir*, *staffagefarve*, *udskydning*. Conservative `altLabels.da` added on 12 terms where a second trade synonym is well established (silketryk, højtryk, flexografi, tampotryk, løbesedler, tværformat/højformat, ryghæftning …). Danish is registered as `llm-draft` in `ptpge:translationStatus` on `/v1/scheme`, pending professional Danish print-domain review. No URIs, envelopes or fingerprints touched.

- `https://ptpge.org/v1/scheme`

## 2026-08-15

### Data: Swedish (sv) — seventh language, full coverage on all 810 terms

Swedish joins as the Standard's seventh language, with complete coverage from day one: **810 labels and 810 definitions** across every branch — Ordering (products, audiences, distribution incl. Incoterms, geography, knowledge, use-cases), Configurations (ISO 216/217 formats, folds, orientation, shape), Substrates, Completions & Features, Management, Variables, Standards (certifications, color systems, file formats, print standards) and Technologies (processes and equipment brands). Terminology follows established Swedish trade usage: *arkoffset*, *rulloffset*, *djuptryck*, *limbindning*, *klammerhäftning*, *bigning*, *foliering*, *bestruket/obestruket papper*, *dekorfärg*. Conservative `altLabels.sv` added on 14 terms where a second trade synonym is well established (serigrafi, högtryck, flexografi, tampotryck, limhäftning, sublimering, radering …). Swedish is registered as `llm-draft` in `ptpge:translationStatus` on `/v1/scheme`, pending professional Swedish print-domain review. No URIs, envelopes or fingerprints touched.

- `https://ptpge.org/v1/scheme`

## 2026-08-12

### Data: German review pass + two definition fixes

A full review of the 112 German definitions added earlier today (Incoterms now use the official German transport-mode phrasing; axis and logistics wording tightened), plus grammar fixes in a handful of older German texts (fold setups, flyers, wire-o). The pass also caught two cross-language defects: **Extra Light Paper's definitions still said 50–79 gsm** in all six languages after this morning's floor move — now 40–79, matching the term's properties; and **Heidelberg (Finishing) described POLAR cutting systems** — it now describes Heidelberg's own Stahlfolder-led postpress portfolio, with POLAR as the separate brand entry introduced today. No URIs, envelopes or fingerprints touched.

- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-weights/extra-light-paper`
- `https://ptpge.org/v1/printing/technologies/printing-finishing-equipment/finishing-equipment/heidelberg-fin`
- `https://ptpge.org/v1/ordering/distribution/incoterms`

### Data: Field-mapping round 1: press-sheet formats, offset split, PUR, guillotine — 810 terms

First harvest from external field mapping: a production house mapped its quotation calculator and full production database (61 + 245 elements) against the API and reported 73–74% coverage — with a precise wish list. This release closes the mechanical, standards-backed part. **Untrimmed press-sheet formats**: the ISO 217 `sra-series` (SRA0–SRA4) and `ra-series` (RA0–RA2) — the sheets printers actually buy, quote and print on. **Newspaper formats**: broadsheet, Berliner and compact (nominal trade dimensions), wired into the newspapers template's envelope. **Offset split**: `sheet-fed-offset`, `heatset-web-offset` and `coldset-web-offset` as narrower terms of offset — mirroring the existing gravure split; the offset URI itself is unchanged. **PUR Binding** under perfect binding. **Guillotine Cutting** — the most basic finishing operation, previously absent. Equipment brands **MBO, Morgana, POLAR**; Stahlfolder recorded as an altLabel of Heidelberg finishing. **Postal Trays** as a packaging term. All 23 new terms carry labels and definitions in all six languages. Configuration count: 3,663,606 → 3,663,702.

- `https://ptpge.org/v1/printing/configurations/formats/sra-series`
- `https://ptpge.org/v1/printing/configurations/formats/ra-series`
- `https://ptpge.org/v1/printing/configurations/formats/newspaper-formats`
- `https://ptpge.org/v1/printing/technologies/printing-technologies/analog/offset`
- `https://ptpge.org/v1/printing/completions/binding/pur-binding`
- `https://ptpge.org/v1/printing/completions/structural-finishing/guillotine-cutting`
- `https://ptpge.org/v1/printing/completions/packaging/postal-trays`
- `https://ptpge.org/v1/ordering/products/newspapers`

### Data: Six languages, complete — every definition real in en · nb · de · es · ja · zh-Hans

Language completion across the whole vocabulary, closing the audit that began with the product templates: all 787 terms now carry a real definition in all six languages. Three parts. (1) Two group nodes — **Features** (`V:FT`) and **Finishing** (`V:FN`) — had no definition in any language, not even English; both are now defined everywhere, with labels to match, and Structural Finishing gained its missing Norwegian. (2) **86 terms lacked de/es/ja/zh-Hans entirely** — the Incoterms, delivery methods, print standards, colour systems, shapes and the whole Variables branch; 349 definitions plus axis labels written. (3) **98 terms only looked translated**: the English text sat under the es/ja/zh-Hans keys (an early-draft fallback). All 294 placeholders replaced with real translations — fold setup models, bindings, laminations, varnishes, foils, Pantone libraries, board materials, audiences and use-cases. Translation status remains LLM-draft pending steward review. No URIs, envelopes or fingerprints touched; configuration counts unchanged.

- `https://ptpge.org/v1/variables`
- `https://ptpge.org/v1/ordering/distribution`
- `https://ptpge.org/v1/ordering/audiences`
- `https://ptpge.org/v1/ordering/use-cases`
- `https://ptpge.org/v1/printing/standards`
- `https://ptpge.org/v1/printing/configurations/setup-models`
- `https://ptpge.org/v1/printing/completions`
- `https://ptpge.org/v1/printing/features`

### Data: Per-surface colour menus completed on the four new templates

Follow-up caught by live validation: colour mode is a per-surface variable in the Standard (outside/inside), and the four templates introduced this week — dust jackets, newspapers, wrapping paper, vinyl graphics — carried only the product-level colour list. They now declare `perSurfaceSupportsColorMode` (and, where the axes exist, per-surface surface-finish and embellishment), mirroring their product-level envelopes — the established pattern. Configuration counts are unchanged; no URIs touched.

- `https://ptpge.org/v1/ordering/products/dust-jackets`
- `https://ptpge.org/v1/ordering/products/newspapers`
- `https://ptpge.org/v1/ordering/products/wrapping-paper`
- `https://ptpge.org/v1/ordering/products/vinyl-graphics`

### Data: Gap analysis round 2: newspapers, wrapping paper, vinyl graphics — 54 templates, 787 terms

A second shop-catalogue sweep (Public Beta window) lands three product classes the Standard lacked. **Newspapers** (`O:PR§NEWSPAPERS`) — folded multi-page on newsprint-style paper, tabloid/A3/custom, carrying the two lightest weight bands. In the same release the **Extra Light band's floor moves 50 → 40 gsm** — an unclaimed edge below the scale — so classic 42–48 gsm web newsprint is expressible too. Nothing reclassifies, and fingerprints are untouched: the canonical form carries the numeric grammage, never the band. Boundary rule set by this change: published band edges only ever move into unclaimed territory; claimed edges never move — a term keeps its meaning forever. **Wrapping Paper** (`O:PR§WRAPPING-PAPER`) — sheets and rolls, the first template to use both the flat-sheet and roll setup models. **Vinyl Graphics** (`O:PR§VINYL-GRAPHICS`) — large-format self-adhesive vinyl, printed and contour-cut: window, wall, floor and vehicle graphics. All three carry labels, definitions and altLabels in all six languages. Plus a small altLabel round from the same sweep: strut cards and table talkers (table cards), gift tags (hang tags), notecards, appointment cards, guest books, wall planners, barrier covers. Totals: **787 terms · 54 templates · 3,663,558** expressible configurations.

- `https://ptpge.org/v1/ordering/products/newspapers`
- `https://ptpge.org/v1/ordering/products/wrapping-paper`
- `https://ptpge.org/v1/ordering/products/vinyl-graphics`
- `https://ptpge.org/v1/ordering/products`
- `https://ptpge.org/v1/search-index.json`

### Data: Binding axes widened, layflat lands, ~150 buyer-word altLabels — 783 terms

A shop-menu gap analysis (Public Beta window) turns into four data changes. **(1) New term** `P:CP§LAYFLAT-BINDING` — the photo-book construction where spreads open completely flat (altLabels incl. *Lay-flat*, *合紙製本*, *蝴蝶装*) — enters the photo-books envelope. **(2) Coil binding reaches five more envelopes** — booklets, books, notebooks, calendars and catalogues now carry `coil-binding`, so “spiral-bound booklet / book / notebook” validates; the binding VALUE composes with any class — no per-variant product templates, ever. Per SD-0047 the US prefLabel stands; “Spiral binding” remains reachable as altLabel. **(3) altLabel harvest** across nine product classes (~150 labels, six languages): genre and buyer words — novels, manga, memoirs, cookbooks, theses, yearbooks, zines, programs, journals, planners, wedding albums, lookbooks, art prints, loyalty cards, wall calendars — resolve to their production class. Genres are content, not configuration: same envelope, same fingerprint space. **(4) SD-0047 outlier fixes:** Swing Tags → *Hang Tags*, Beer Mats → *Coasters*, Pavement Signs → *Sidewalk Signs* (UK names kept as altLabels; URIs untouched — identifiers never change). **(5) Six languages, fully:** Spanish, Japanese and Chinese definitions completed across all 51 product templates (135 translations) — every template now carries labels, definitions and altLabels in all six languages of the Standard. **(6) SD-0047 UK-spelling pass closed:** UK variants verified as altLabels across every label in the vocabulary (two additions — *Spot Colour*, *Colour Profiles*; proper names such as Japan Color are exempt). Also recorded: letterheads gained the classic Light Paper band (80–109 gsm) on 2026-08-10. Totals: **784 terms**; the 51 templates now describe **3,663,302** expressible configurations (full envelope).

- `https://ptpge.org/v1/printing/completions/binding/layflat-binding`
- `https://ptpge.org/v1/printing/completions/binding/coil-binding`
- `https://ptpge.org/v1/ordering/products/booklets`
- `https://ptpge.org/v1/ordering/products/photo-books`
- `https://ptpge.org/v1/ordering/products/letterheads`
- `https://ptpge.org/v1/search-index.json`

## 2026-08-10

### Data: Nine harvested product terms withdrawn (Public Beta window) — products and product templates now 1:1 (782 terms)

Nine product-level terms are **withdrawn before stabilization**: `O:PR§3D-FIGURES`, `O:PR§C5`, `O:PR§CHAIRS`, `O:PR§CHROMALUXE`, `O:PR§CUSTOMIZED`, `O:PR§GICLEE`, `O:PR§MODELS`, `O:PR§PRINTS`, `O:PR§T-SHIRTS`. They were catalogue harvests from the v1/v2 build phase — names from shop navigation, not product classes — and their meaning already lives in the correct branches: C5 as *formats* (`formats/c-series/c5`, `formats/international-envelopes/env-c5`), giclée and 3D as *technologies/substrates*, “customized” as a configuration value. All nine were status `proposed`, none carried an envelope, none was listed in the products index, and no fingerprint, register event, beta integration or published artifact references them — verified before removal. This withdrawal happens inside the Public Beta window, before the permanence guarantee binds at opening; after it, identifiers are never retired. A future textile class (e.g. t-shirts) would enter as a *new* term with a proper envelope, not as a resurrection. Effect: ordering products 59 → **50, every one carrying a Configuration Envelope** — the vocabulary and the product-template count now agree 1:1. Totals: **782 terms**.

- `https://ptpge.org/v1/ordering/products`
- `https://ptpge.org/v1/printing/configurations/formats/c-series/c5`
- `https://ptpge.org/v1/printing/configurations/formats/international-envelopes/env-c5`
- `https://ptpge.org/v1/printing/technologies/printing-technologies/digital/inkjet/giclee`
- `https://ptpge.org/v1/printing/technologies/printing-technologies/digital/3d`
- `https://ptpge.org/v1/printing/substrates/object-materials/3d`
- `https://ptpge.org/v1/search-index.json`

## 2026-07-18

### Data: Sustainability proof & approval states — carbon accounting, climate-documentation axis, DPP anchor, proof-approval (791 terms)

Eleven terms land, closing the vocabulary side of the *machine-readable proof* demand in agent-driven procurement (an agent that filters on a CO₂ proof needs URIs to filter on). **(1) Carbon accounting** — new sub-branch `printing/standards/certifications/environmental/carbon/` with **ISO 14067**, the **GHG Protocol Product Standard** and **ClimateCalc** (the print industry’s own calculator framework): the recognized methods behind product-level CO₂e declarations. The declared figures themselves stay in offers and vendor profiles — like price, they are claims, not configuration, and are excluded from `ptpge-cfs/1`. **(2) A buyer-side requirement axis** `variables/compliance/climate-documentation` (enum, single, valueDomain → the carbon branch) makes “climate-documented only” expressible in a configuration. **(3) Digital Product Passport** — new `printing/standards/regulations/` branch with a DPP reference term as the regulatory anchor (EU ESPR) for machine-readable sustainability data. **(4) Proof approval** — `printing/management/quality-assurance/proofing/proof-approval` with three states (`proof-submitted`, `changes-requested`, `proof-approved`), giving commerce-protocol extensions (e.g. a ProofApproval capability) stable URIs for the approval loop. All terms `proposed`, six languages. Totals: **791 terms**, dataVersion 2026-07-18.

- `https://ptpge.org/v1/printing/standards/certifications/environmental/carbon`
- `https://ptpge.org/v1/printing/standards/certifications/environmental/carbon/iso-14067`
- `https://ptpge.org/v1/printing/standards/certifications/environmental/carbon/climatecalc`
- `https://ptpge.org/v1/variables/compliance/climate-documentation`
- `https://ptpge.org/v1/printing/standards/regulations/digital-product-passport`
- `https://ptpge.org/v1/printing/management/quality-assurance/proofing/proof-approval`
- `https://ptpge.org/v1/search-index.json`

## 2026-07-16

### Identifiers: ptpge-cfs/1 reference material reconciled to the live variable taxonomy (SD-0040)

The fingerprint *reference material* is brought back in line with the deployed reality. The commercial-cluster prefix in the reference implementation and the SD-0040 spec was still the retired pre-release namespace `variables/commercial/` (with the equally retired `structure/`, `dimensional/`, `color/` and `embellishment/` clusters in the worked example), while the live server, the configurator and the published vocabulary have long agreed on `variables/order/`. **Production behaviour was never wrong** — the staleness was confined to reference artifacts, exactly the pre-`approved` breakage §10 of the spec provides for. Reconciled: `tools/fingerprint.py` (prefix, `KNOWN_ORDERED`, worked example), the spec’s profile table and §9 example, and the SD-0040 decision text. **New reference vectors** `f2e377cbb004c4e6…` (spec) / `ad112a406169f743…` (offer) are registered in the public resolver; the superseded vectors `ab469f126af8cdd7…` / `e18441aacc2f9851…` remain resolvable as historical pages — fingerprints are never retired. A **three-way conformance test** (reference implementation = live server = configurator, plus `KNOWN_ORDERED` ≡ vocabulary `ordered` flags and full resolver-catalogue integrity) now guards this drift class in `tests/`.

- `https://ptpge.org/f/f2e377cbb004c4e628180d877f8b1e008a7055e5d699140ecb8f3bf0a73f5df1`
- `https://ptpge.org/f/ad112a406169f743a1f6d9089b34bdc27ae681e280a689a658d56108a24ab600`
- `https://ptpge.org/f/ab469f126af8cdd710c153fb8a490b44654dc226adcbf0174c5a085e730f8b54`
- `https://ptpge.org/f/e18441aacc2f98515c2377104ab4d6923d1d4062094681428bdab65b6bf936b8`
- `https://ptpge.org/v1/variables/order/quantity`

## 2026-07-13

### Identifiers: GutenLex Scan (beta) — ptpge.org/scan

A camera PWA that reads a fingerprint QR — and, in beta, the fingerprint mark itself — and resolves the identity on the permanent pages. Nothing leaves the device except the lookup of the fingerprint. Works on desktop and mobile; updates itself.

- `https://ptpge.org/scan/`

## 2026-07-12

### Identifiers: Permanent fingerprint pages: ptpge.org/f/{hash}

Every registered configuration fingerprint now resolves on the neutral PTPGE namespace: `ptpge.org/f/{hash}` answers as HTML for people and JSON for systems (content negotiation), renders the canonical `ptpge-mark/2` mark, and returns an honest 404 for unknown hashes. Fingerprints are permanent and never retired.

- `https://ptpge.org/f/`

## 2026-07-02

### Data: Definition pass across the vocabulary

Roughly 125 terms received refined English and Norwegian definitions and label polish. No URIs changed; no terms were added, moved or deprecated.

- `https://api.ptpge.org/v1/`

## 2026-07-01

### Policy: Public Beta opens — invite-only access to the full Layer 1 surface

As of **1 July 2026** the PTPGE Standard runs as an invite-only **Public Beta**. What that means for consumers: the REST API (`api.ptpge.org`) and the MCP server (`mcp.ptpge.org`) are access-controlled — apply for credentials at [register.ptpge.org](https://register.ptpge.org/) (developer, supplier or partner role; applications are reviewed by hand). An **open, unauthenticated mirror** remains available at `internal.ptpge.org` and `open.mcp.ptpge.org` for tools that cannot carry auth. The contract during the Public Beta: the `/v1/` URI scheme is **locked**, term content (labels, definitions, properties) may still evolve, and changes are governed by the published [Deprecation Policy](/v1/deprecation-policy.html). Term data will be released under **CC BY 4.0** at public launch. Preview pages, demos and the registration flow are available in 13 languages; the term corpus itself carries 6 full language sets (en, nb, de, es, ja, zh-Hans) plus a growing Dutch core.

- `https://api.ptpge.org/v1/`
- `https://register.ptpge.org/`
- `https://api.ptpge.org/v1/deprecation-policy`

## 2026-06-26

### Data: Geography aligned to ISO 3166-1, BeNeLux added, and a new checkout/distribution facet (data 2026-06-26)

**Geography → ISO 3166-1.** Every country is now anchored to its ISO country code (alpha-2 + alpha-3) in `properties`, with the codes added to `altLabels` so they are searchable (e.g. `BE`, `NL`, `US`). A new **BeNeLux** sub-region (Belgium, Netherlands — moved in — and Luxembourg) joins Nordic and DACH; European coverage broadened to 60 countries in total, and the United States label dropped its inline “(US)”. **New `distribution` group (Ordering Process).** Two categories: Delivery Methods (standard, express, courier, freight/pallet, pickup, postal) and the full set of Incoterms® 2020 (EXW … DDP, referenced to the ICC — not redefined). **Three new order variables:** `delivery-method`, `incoterm` and `lead-time` — order-level axes that enter the offer fingerprint (like quantity). Note: the Netherlands URI moved under BeNeLux (`…/europe/benelux/netherlands`); the term was `proposed`, pre-GA.

- `https://ptpge.org/v1/ordering/geography/europe/benelux`
- `https://ptpge.org/v1/ordering/distribution`
- `https://ptpge.org/v1/ordering/distribution/incoterms`
- `https://ptpge.org/v1/variables/order/delivery-method`
- `https://ptpge.org/v1/variables/order/incoterm`
- `https://ptpge.org/v1/variables/order/lead-time`

## 2026-06-23

### Structure: Product catalogue overhaul: 50 buyer-facing products, full envelopes, new Setup Models, and a Print + Large-Format scope (data 2026-06-23)

A major build-out of the Ordering Process product catalogue. **Scope (SD-0051):** the catalogue focuses on traditional print — *offset, flexo and digital* — plus *large-format* technology. Pure *merchandise* (mugs, apparel, tote bags, phone cases, drinkware, pens, home & living) and *3-D* products are out of scope; `3d-figures`, `chairs` and `t-shirts` were removed and the trademark `ChromaLuxe` retired (metal print is a substrate/technique). **50 products**, flat under `ordering/products` (SD-0042), each now carrying a real buyer-facing definition (en/nb/de), enriched altLabels, a Setup Model and a full Configuration Envelope (`supports*` / `perSurfaceSupports*`). **Four new Setup Model families** join Flat/Folded/Multi-page: *Structural* (die-cut packaging — boxes, cartons, bags), *Roll* (labels/stickers on reel), *Multi-part* (carbonless/NCR), and *Display System* (wide-format graphic + hardware). Principle: **Setup Model is topology, not technology** — wide-format products that are topologically flat (banners, rigid signs, posters, canvas) stay on Flat Sheet and express their wide-format nature via Technology/Substrate. **Vocabulary:** *Giclée* reclassified from product to inkjet technology; new *Fine Art prints* product; *Wood* substrate split into Solid (edge-glued panel) and Composite (plywood, OSB, particleboard, MDF, fiberboard); *Aluminium* split into Solid Aluminium and Composite Aluminium (ACM/Dibond). **altLabels pass** for prompt/agent resolution: formats (dimension + cross-system aliases — DIN A4, 11x17, Letter/Tabloid/Ledger), standards (Pantone/PMS, ICC, ISO 12647/Fogra/GRACoL/SWOP, FSC/PEFC, JPEG/PNG/PDF/EPS), and management workflow concepts.

- `https://ptpge.org/v1/ordering/products`
- `https://ptpge.org/v1/printing/configurations/setup-models`
- `https://ptpge.org/v1/printing/substrates/board-materials`

## 2026-06-19

### Structure: Completions & Configurations axis cleanup: Surface Finish, Edge, Orientation/Shape, Setup Models, Page Count (data 2026-06-19)

Continuing the axis-separation work (SD-0050), several Completions and Configurations categories that conflated orthogonal axes or duplicated other axes are cleaned up. **Surface Finish** is now populated with both *Varnish* and *Lamination* (laminations moved from `completions/finishing` into `features/surface-finish`); the Surface Finish variable points there directly. Under **Completions**, *Folding* is promoted to a top-level category and the redundant *Finishing* branch is removed (its leaves duplicated Binding / Structural Finishing). A shared edge axis **Fold & Binding Edge** (`Left/Right/Top/Bottom`, picked by the Edge variable) replaces the old per-edge naming — the specific fold/bind type stays on the Folding/Binding axes. Under **Configurations**, the conflated *Orientations & Shapes* is split into two independent axes — **Orientation** (Portrait/Landscape/Square) and a new **Shape** axis (Rectangle/Circle/Ellipse/Rounded Rectangle/Freeform) with its own Shape variable. **Setup Models** is collapsed to three coarse archetypes (Flat Sheet / Folded Sheet / Multi-page) that gate which other axes apply; the 13 fold-setup and the booklet-setup duplicates are removed. **Page Count** moves from the Order to the Configurations cluster (a product-structure axis, not an order quantity; `V:OR§PAGE-COUNT` → `V:CF§PAGE-COUNT`), remaining a numeric literal. **Colour modes:** *CMYKOG* (6-colour) added and *CMYK+OGV* renamed to *CMYKOGV* (both `exactMatch` their colour spaces); the *Metallic*, *Opaque* and *Custom Color* modes are removed. **URI change (Stealth window):** the affected term URIs change (e.g. `configurations/orientations-shapes/*` → `configurations/orientation/*` + new `shape/*`; `completions/folding-binding-placements/*` → `completions/fold-binding-edge/*`); permissible per SD-0023 (all terms `proposed`, no public integrators pinned). Product envelopes and configuration fingerprints that referenced the changed URIs are updated accordingly.

- `https://ptpge.org/v1/printing/configurations`
- `https://ptpge.org/v1/printing/completions`

## 2026-06-18

### Structure: Colour area restructured: colour VALUES split from colour SYSTEMS (data 2026-06-18)

The colour domain is split along a clear boundary (SD-0049): **`features/colors`** now holds only the colour *modes* a buyer selects (Black, White, CMYK, CMYK+OGV, Metallic, Opaque, Custom Color, None, and a new generic **Spot Color**), while **`standards/color-systems`** holds the reference frameworks that define, encode, match or measure colour, in four branches: *Color Spaces* (now incl. CMYKOG, CMYKOGV), *Color Matching Systems* (Pantone + SMS, HKS, RAL, Toyo), a new *Color Profiles* branch (ICC Profiles, Device Profiles), and a new *Vendor Ink Ecosystems* branch (Hexachrome, Touch 7, Riso, Seven-Color, plus Epson UltraChrome, HP IndiChrome, EFI Fiery XF). Thirteen terms were created and nine moved from Features to Standards, with `elementType` changed from `value` to `reference`; `exactMatch` links connect the dual-nature concepts (CMYK mode ↔ CMYK space; CMYK+OGV ↔ CMYKOGV). The **Color/ICC overlap** is resolved: the ICC Profiles *reference* lives under `standards/color-profiles`, while `management/quality-assurance/color-management` keeps the *workflow* (Color Management, Adobe Color Management) and `related`-links to the reference; the duplicate was removed. **URI change (Stealth window):** nine colour terms move from `features/colors/…` to `standards/color-systems/…`; permissible per SD-0023 (all terms still `proposed`, no public integrators pinned). The *Color Mode* and *Color Profile* variable value-domains keep their paths (slimmer Features, richer Standards); configuration fingerprints whose tokens reference these recompute accordingly.

- `https://ptpge.org/v1/printing/features/colors`
- `https://ptpge.org/v1/printing/standards/color-systems`

## 2026-06-17

### Structure: Variables realigned + Products flattened; Print Standards added (data 2026-06-17)

Layer 1 **Variables** (the configurable axes) restructured so each cluster mirrors the Printing Process group its values live in (SD-0048): seven clusters — `V:ST` Compliance, `V:TC` Production, `V:CF` Configuration, `V:FT` Features, `V:SB` Substrate, `V:CP` Finishing, `V:OR` Order. The *Die Cutting* and *Perforation* axes are merged into one multi-select **Structural Finishing** variable; *Fold*→*Folding*, *Fold Direction*→*Edge*. Missing value-domains filled (Binding → `completions/binding`; Structural Finishing → `completions/structural-finishing`). New **Print Standards** category under Standards with nine conformance standards (ISO 12647-2/-7, G7, GRACoL 2013, SWOP, PSO, Japan Color 2011, Fogra39, Fogra51) wired to the *Print Standard* variable. English standardised to **US spelling** as canonical with UK spellings added as `altLabels` (SD-0047). **URI change (Stealth window):** variable URIs and notations change (e.g. `V:DM§FORMAT` → `V:CF§FORMAT`); permissible per SD-0023 because all terms are still `proposed` and no public integrators are pinned. Configuration fingerprints whose tokens reference these variables recompute accordingly. No Printing value URIs changed. **Products flattened (SD-0042):** the `ordering/products/` catalogue is flattened to 38 product classes directly under `products` — the `/configurable/` level and the category buckets (sheets, boards, expo …) are removed, grouping moves to facets. Material entries that duplicated substrate values (boards: acrylic/metal/wood …; rolls: vinyl/foil …) are deleted — a product's material is chosen via the Substrate Type variable. Ordering-mode entries (pre-designed, quote-based) are removed from Layer 1 (they are Layer 2/4 concerns). Product URIs change (`ordering/products/configurable/sheets/brochures` → `ordering/products/brochures`); the UPT crosswalk targets are updated in lockstep and configuration-fingerprint product tokens recompute.

- `https://ptpge.org/v1/variables`
- `https://ptpge.org/v1/printing/standards/print-standards`

## 2026-06-06

### Data: Data version advanced to 2026-06-06 (finalises the Configurable product work)

Maintenance release. The published data version is advanced from `2026-06-05` to `2026-06-06` to match the actual last-modified date of the *Configurable* product terms finalised under the 2026-06-05 entries. The seed product **Brochure** now carries its full **Configuration Envelope** — the `supports*` and `perSurfaceSupports*` predicates the Layer 2 Configurator reads to validate a configuration and emit its `ptpge-cfs/1` fingerprint. Additive maintenance — no term URIs changed in this increment.

- `https://ptpge.org/v1/ordering/products/configurable/sheets/brochures`

## 2026-06-05

### Structure: Product channel renamed: Buy Online → Configurable (O:PR-BUY → O:PR-CFG)

The product channel formerly labelled **Buy Online** is renamed **Configurable** — the products configured and ordered directly online via their Configuration Envelope (Variables), as opposed to *Pre-Designed* (ready-made) and *Quote-Based* (custom RFQ). The name now describes the defining property (these are the product classes that *have* an envelope and a fingerprint) rather than a sales channel. **URI change (Stealth window):** the path segment `ordering/products/buy-online/…` becomes `ordering/products/configurable/…` for ~68 product terms, and the notation prefix `O:PR-BUY` becomes `O:PR-CFG`. Per SD-0023 this is permissible only because all terms are still `proposed` and no public integrators are pinned; it would not be allowed after GA. **Fingerprint impact:** configuration fingerprints whose product token sits under this path change accordingly — the spec § 9 worked example is recomputed (spec `ab469f12…`, offer `e18441aa…`); the reference implementation, MCP server tests and UPT crosswalk targets are all updated in lockstep. No labels, definitions or structural relations of any product term changed.

- `https://ptpge.org/v1/ordering/products/configurable`
- `https://ptpge.org/v1/ordering/products/configurable/sheets/brochures`

### Data: Synonym dimension introduced — altLabels populated on 93 buyer-relevant terms (en + nb)

The `altLabels` field — defined in the schema since v1 but populated on zero terms — receives its first curated content: **93 buyer-relevant terms get alternative labels in English and Norwegian**, focused on how print buyers actually phrase things rather than how the industry names them. Examples: *Full Color / Fullfarge / 4/4* → CMYK; *Kun sort / Black Only* → Black; *Trekkspill / Leporello / Concertina Fold* → Accordion Fold; *Gultonet papir / Cream Paper* → Ivory Paper; *Litt tykt papir* → Medium Paper (150–170 gsm); *Opplag / Eksemplarer / Print Run* → the Quantity variable; *Gramvekt / GSM / Grammage* → the Substrate Weight variable. The Norwegian trade term **fals/falsing** is added across the folding family. Coverage: color layers, foil stamping and varnishes, folding values and Setup Models, A3–A6 formats, paper color shades / surfaces / weight bands, laminations, 22 products (incl. *løpesedler*, *postkort*, *ølunderlag*) and 15 Variables. altLabels flow into `search-index.json` and are searched by the MCP server, whose search was simultaneously upgraded to tokenized, language-agnostic matching (server v0.3.2) — free-text buyer queries in either language now resolve to canonical URIs. **Also in this release:** the eight Paper Weight bands gain machine-readable `properties` (`minWeight`/`maxWeight`/`unit: "gsm"`) and are redefined as **contiguous** ranges (50–79, 80–109, 110–149, 150–179, 180–239, 240–309, 310–449, 450+) so every numeric `substrate-weight` binding from 50 gsm up resolves to exactly one band; and a cross-language label audit against English as lead language corrected 47 labels in 23 terms (envelope dimensions missing in es/zh, CIE ranges missing on paper color shades, stale weight ranges, a copy-paste error on 2A0). No URIs changed.

- `https://ptpge.org/v1/printing/features/colors/cmyk`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-color-shades/ivory-paper`
- `https://ptpge.org/v1/printing/completions/finishing/folding/accordion-fold`
- `https://ptpge.org/v1/variables/commercial/quantity`
- `https://ptpge.org/v1/search-index.json`

## 2026-06-04

### Structure: Part/Surface model + one-product-per-class consolidation (SD-0037, SD-0038)

Two structural decisions land together. **(1) The Part/Surface model (SD-0037).** The spec-unit terminology is unified on **Surface** — an *addressable print face* defined by the Setup Model (Front/Back for flat sheets, Outside/Inside Panels for folded sheets, and grouped faces like Outside Pages for multi-page products). The former "Section" wording is retired across all six languages — it collides with bindery terminology, where a section is a folded signature. Product fields are renamed `perSectionSupports*` → `perSurfaceSupports*`, the variable scope enum becomes `product / part / surface` (with **Part** — cover/body — reserved for compound products; the Setup Model defines the part/surface topology), and `surface-finish` becomes `cardinality: multiple` (an ordered stack of up to two finish layers per surface). **(2) One product per product class (SD-0038).** The 13 pre-composed brochure fold variants are consolidated into a single **Brochure** product (`O:PR§BROCHURE`, same URI as before). Fold topology is now a variable binding — the new `supportsSetupModel` field lists the 13 supported topologies — and panel/page count is carried by the new **page-count** variable (`V:ST§PAGE-COUNT`, numeric; the 22nd Variable). Validity rules (panel counts per topology, multiples of 4 for saddle-stitching) are Layer 2 (Configurator) logic, per the granularity principle. Business Card was already modelled this way; the corpus now follows one consistent pattern: *product identity in Layer 1, configuration via Variables, legality in Layer 2* — exactly the shape an agentic Product Configuration payload (UCP / dev.ucp.print) composes. **Totals:** 708 terms (13 variants archived pre-publication — no tombstones needed), 22 Variables, build clean, schema scope-enum updated.

- `https://ptpge.org/v1/ordering/products/buy-online/sheets/brochures`
- `https://ptpge.org/v1/variables/structure/page-count`
- `https://ptpge.org/v1/variables/embellishment/surface-finish`
- `https://ptpge.org/v1/printing/configurations/setup-models`
- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/terms.jsonld`

## 2026-05-30

### Structure: Variables top concept + axis-typed Notation v3.0 + elementType (SD-0033) — pre-First-Proof release

The pre-launch release for **First Proof** (Open Beta). Three changes land together. **(1) Variables — a third top concept.** `/v1/variables/` joins Printing Process and Ordering Process. It holds the **configurable axes** of a printed product — 8 clusters (dimensional, substrate, color, structure, embellishment, production, compliance, commercial) and **21 Variables** (Format, Color Mode, Fold, Substrate Colour, Substrate Weight, Quantity, Setup Model, …). Each Variable carries machine-readable metadata — `datatype` (enum/numeric), `cardinality`, `unit`, `scope` (product/per-surface) — and a `valueDomain` that references the existing values it draws from. **No values were moved**: Variables is a thin registry overlay, and it is the connective layer (the "bridge") both sides point into — the substrate that makes agentic configuration possible (UCP / dev.ucp.print). Valid combinations and cross-variable rules remain Layer 2 (PTPGE Configurator), not here. **(2) Notation v3.0 — axis-typed.** Every notation now leads with the top-concept axis and a colon: `P:` (Printing), `O:` (Ordering), `V:` (Variables). Delimiter grammar is one-character-one-meaning: `:` = axis boundary, `-` = structural step, `§` = structural/leaf boundary. Examples: `P:CP-FIN-FOLD`, `P:CF§A4`, `O:PR§TRI-FOLD-BROCHURE`, `V:DM§FORMAT`. **The Ordering side now carries notation** (amends SD-0020's printing-only scope); 666 term files were re-notated. Notation is not the API lookup key (URIs are unchanged), so this is non-breaking for URL-based consumers. **(3) elementType — a cross-cutting kind.** A new optional field on leaf elements, orthogonal to the hierarchy, telling consumers how to treat each element: `value` / `variable` / `product` / `reference` / `place` / `context` / `resource`. Set on 539 elements; optionally surfaced as the periodic-table colour category. **Also:** the Fold duplication between Completions (fold operation) and Setup Models (fold topology) was resolved by correcting the intra-scheme `exactMatch` links to `skos:related` — they are related, not identical; no concepts deleted, the product model is intact. **Totals:** 696 terms, build clean, schema (notation regex v3.0 + `elementType` enum + Variables metadata) and the JSON-LD context updated. Migration scripts under `tools/migrate_*` and `tools/generate_variables_*`.

- `https://ptpge.org/v1/variables`
- `https://ptpge.org/v1/variables/dimensional/format`
- `https://ptpge.org/v1/variables/substrate/substrate-colour`
- `https://ptpge.org/v1/variables/commercial/quantity`
- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/terms.jsonld`

## 2026-05-27

### Data: Complete language coverage on all leaf-term files — 100% label + definition coverage across the entire 667-term corpus (SD-0030)

Extends SD-0029 from `_index` files to the entire term corpus. All 542 leaf-term JSONLD files now have **complete label and definition in all 6 supported languages** (en, nb, de, ja, es, zh-Hans), bringing the total PTPGE corpus to **100% language coverage on labels and definitions across all 667 terms** (542 leaf + 125 `_index`). **Translation strategy**: (1) Pass-through (~250 terms) — brand names (HP, Heidelberg, Komori, Adobe, Pantone, Hahnemühle, Lessebo Scandia, etc.), format codes with dimensions (A4, B2+, Envelope C5), ISO standards (ISO 12647, ISO 9001, ANSI), and file format codes (.pdf, .ai, .svg, PDF/X) are identical across all languages — same string for every language as is bransje-standard. (2) Explicit translations (~270 terms) — descriptive terms across 9 groups received dedicated translations using established industry vocabularies: fold patterns (Single Fold, Tri Fold, Z-Fold, Accordion), binding (Saddle Stitching, Perfect Binding, Wire-o), colours (Black, White, Gold Foil, Holographic Foil), countries (Norway, Germany, Japan, etc.), product types (Brochures, Posters, Wine Boxes, Door Hangers), printing processes (Lithography, Flexo, Letterpress, Engraving, Mezzotint), substrates (Cardstock, Vinyl, Glass, Wood, paper weight tiers). (3) Definition supplements (~150 terms) — files with English definitions but missing other language entries received LLM-draft stubs that copy the en source; marked as draft via `langStatus` on parent groups. (4) Definition templates (~120 terms) — files with no definition received group-specific templates: Geography countries get a structural facet description; Products get a Setup-Model-reference template; Knowledge guides/tools/templates get role-specific templates that explicitly cross-link to Layer 1 via skos:related. **Verification:** orphan check clean, build pipeline runs cleanly, 0 incomplete labels and 0 incomplete definitions across the entire corpus. **Known follow-up:** LLM-draft definitions (most ja/es/zh-Hans entries on leaf terms) await human review or dedicated translation pipeline; structural completeness is in place but content quality varies between production-tier (en/nb/de established) and draft-tier (ja/es/zh-Hans LLM-assisted).

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/terms.jsonld`
- `https://ptpge.org/v1/ordering/geography/europe/norway`
- `https://ptpge.org/v1/ordering/geography/asia/japan`
- `https://ptpge.org/v1/printing/configurations/setup-models/folded-sheet/tri-fold`
- `https://ptpge.org/v1/printing/technologies/printing-technologies/analog/lithography`
- `https://ptpge.org/v1/printing/substrates/sheet-materials/paper/paper-weights/heavy-paper`

### Data: Complete language coverage on all _index files — every group/category/sub-category navigable in all 6 supported languages (SD-0029)

All 125 `_index.jsonld` files across `terms/printing/` and `terms/ordering/` now have **complete label + definition coverage in all 6 supported languages** (en, nb, de, ja, es, zh-Hans). Extends SD-0026 (which covered the 6 top-level ordering Groups) to the entire `_index`-file space. **110 files updated**: ~80 printing-side files received ja/es/zh-Hans labels (definitions were already complete in 6 languages); ~25 ordering-side depth nodes received full label + definition content where they previously had only English labels (Geography subregions, Knowledge categories, Buy Online product family subcategories); and ~5 files with partial 3/6 coverage received supplement ja/es/zh-Hans entries. Per the SD-0026 `langStatus` convention, en/nb/de remain production-tier and ja/es/zh-Hans remain draft-tier. Geographic subregion definitions now reference concrete country codes (ISO 3166-1 alpha-2): Nordic = NO/SE/DK/FI/IS, DACH = DE/AT/CH, North America = US/CA/MX, etc. Buy Online product family subcategories (Boards, Binders, Envelopes, Expo, Fine Art & Photo, Labels & Stickers, etc.) received a programmatic generic definition that names the family and explains its role within the Buy Online procurement channel — to be replaced with specific definitions as each product-family model matures. **No breaking changes** — all additions purely fill in missing fields. **Known follow-up:** child terms (~137 in ordering, several hundred in printing) remain partially translated and are out of scope for SD-0029.

- `https://ptpge.org/v1/ordering/geography/europe/nordic`
- `https://ptpge.org/v1/ordering/geography/europe/dach`
- `https://ptpge.org/v1/ordering/knowledge/guides`
- `https://ptpge.org/v1/ordering/products/buy-online`
- `https://ptpge.org/v1/printing/features/embellishments`
- `https://ptpge.org/v1/printing/configurations/setup-models/folded-sheet`
- `https://ptpge.org/v1/printing/technologies/printing-technologies`

### Structure: Hierarchy review: Setup Models → Printing/Configurations, Layer 2 renamed to Configurator, additive scopeNotes + cross-links (SD-0028)

After a structured review of the v3 hierarchy across 8 dimensions, several refinements land in one batch. **Setup Models migration:** the Setup Models subtree moves from `/v1/ordering/setup-models/` to `/v1/printing/configurations/setup-models/`, joining Formats and Orientations & Shapes as siblings under Configurations. The placement reflects that Setup Models describe production topology (producer-side), not buyer-side procurement. 14 Product Templates updated to reference the new `setupModel` URIs. Old URIs removed (no formal deprecation cycle required since `MATURITY = "stealth"` and no public integrators reference the old paths). **Configurations promotion to top concept (option C) explicitly reserved** with five trigger conditions documented in SD-0028 §10. **Layer 2 renamed:** "PTPGE Configuration" → "PTPGE Configurator" (engine, not output) across dashboard pages and documentation; eliminates name collision with the Layer 1 *Configurations* Group while matching the existing `configurator.ptpge.org` subdomain. **Additive scope-notes and cross-references:** Standards _index gets a multilingual `scopeNote` documenting umbrella semantics (Measurement Systems, Paper Size Standards, Graphic File Formats, Color Systems, Certifications); Products _index documents that narrower terms are procurement channels (Buy Online / Pre-Designed / Quote-Based), not product types; reciprocal `skos:related` + scopeNotes between Color Systems (Standards) and Print Colors (Features) document the intentional split between color vocabularies and applied colors. **Knowledge ↔ Layer 1 cross-links:** 15 Knowledge Guides / Tools / Templates get `skos:related` pointers to the Layer 1 terms they explain (e.g., "Folding Types & Creasing" → `/v1/printing/completions/folding-binding-placements`). **schema.org closeMatch:** 4 Audiences (b2b, b2c, consumer, educational) and 5 Use Cases (educational, event-promotion, informational, instructional, marketing) get `skos:closeMatch` to schema.org URIs to enable interoperability with web/AI systems that recognise schema.org natively. All non-Setup-Models changes are purely additive — no breaking changes for any consumer that already integrated against the API.

- `https://ptpge.org/v1/printing/configurations/setup-models`
- `https://ptpge.org/v1/printing/standards`
- `https://ptpge.org/v1/printing/standards/color-systems`
- `https://ptpge.org/v1/printing/features/colors`
- `https://ptpge.org/v1/ordering/products`
- `https://ptpge.org/v1/ordering/knowledge/guides`
- `https://ptpge.org/v1/ordering/audiences`
- `https://ptpge.org/v1/ordering/use-cases`

### Policy: API Deprecation Policy v0.9 — Beta published as live endpoint (SD-0027)

The PTPGE API now has a published, maturity-tiered Deprecation Policy at `/v1/deprecation-policy` (HTML) and `/v1/deprecation-policy.json` (machine-readable). The policy makes the API contract explicit: which URIs are stable, how breaking changes are signalled, and how long deprecation windows last. **Commitments are calibrated to the current operational maturity** — beta-tier values (e.g., 9-month deprecation window, 12-month v1→v2 sunset, best-effort SLAs) apply now; production-tier values (18-month deprecation, 24-month sunset, 5-day acknowledgement SLA) apply automatically when `MATURITY = "production"`. The policy itself describes the graduation criteria in §10 and lists the current Maintainers in §11. Complementary infrastructure also lands: `docs/VERSIONING.md` (v1→v2 migration playbook), `tools/scaffold_v2.py` (one-way scaffold generator for a future v2 working tree — gated behind `--force`), and `docs/PRE-OPEN-BETA-CHECKLIST.md` (structured review checklist gating the `beta → production` transition). The API root landing page now carries a *⚖ Deprecation Policy* pill linking directly to the policy, and the policy page has its own topbar with *← API home* back-navigation matching the term-page style.

- `https://api.ptpge.org/v1/deprecation-policy`
- `https://api.ptpge.org/v1/deprecation-policy.json`
- `https://api.ptpge.org/v1/version.json`

### Data: Ordering groups gain full multilingual coverage + new langStatus field (SD-0026)

All six `_index.jsonld` files under `/v1/ordering/` now have `definition` populated in all six supported languages (en, nb, de, ja, es, zh-Hans). **27 new definitions added** across the six groups — full set for the three that previously had none (`geography`, `knowledge`, `products`), plus the missing ja/es/zh-Hans entries on the three that already had en/nb/de (`audiences`, `setup-models`, `use-cases`). A new top-level field `langStatus` is introduced on each `_index`: a per-language map declaring quality tier — `"production"` for en/nb/de and `"draft"` for ja/es/zh-Hans (matching the existing asterisk-marked language convention in HTML rendering, but now machine-readable for API consumers). The field is additive; consumers ignoring it see no change. Schema validation is unchanged (top-level `additionalProperties` remains permissive). **Also corrected:** `STATUS.md` previously stated *3 groups* under ordering; the actual structure has *6 groups* (audiences, geography, knowledge, products, setup-models, use-cases) and is now documented as such. **Known follow-up:** `schemas/ptpge-term.schema.json` `group` enum still only lists 3 of the 6 ordering groups — child terms in audiences/setup-models/use-cases will fail validation if added before this is patched.

- `https://ptpge.org/v1/ordering/audiences`
- `https://ptpge.org/v1/ordering/geography`
- `https://ptpge.org/v1/ordering/knowledge`
- `https://ptpge.org/v1/ordering/products`
- `https://ptpge.org/v1/ordering/setup-models`
- `https://ptpge.org/v1/ordering/use-cases`

## 2026-05-20

### Structure: Notation v2.0 — Printing-only scope, § separator, hierarchical-structural / flat-leaf (SD-0020)

The notation system is restructured per **SD-0020**. Three rules: (1) **Printing Process only** — Ordering Process terms (products, audiences, geography) no longer carry notations; they're referenced by URI or label. ~280 OP-* notations removed. (2) **Two separators:** dash (`-`) for structural levels in hierarchical form, silcrow (`§`) for leaf elements in flat form. (3) **Length-by-level for structural:** group 2 chars (`CF`, `CP`, `ST`, `MG`, `TC`, `FT`, `SB`); category 3 chars (`CF-FMT`, `CP-FIN`); sub-category 4 chars (`CF-FMT-ASER`, `CP-FIN-LAMI`); deeper levels add 4-char segments. **Leaves flat:** `CF§A4`, `CP§GLOSS`, `FT§CMYK`, `ST§ISO12647-7`, `SB§SCANDIA-SMOOTH-IVORY` (brand-prefixed). **Collision fixes:** 20 leaf slugs renamed to be globally unique — international envelopes get `env-` prefix (env-b4, env-c5, etc.) to distinguish from paper formats; vendors on multiple equipment lines get category suffix (heidelberg-press vs heidelberg-fin, hp-press vs hp-wf, esko-ae-pf vs esko-ae-wf); Lessebo Scandia colour variants get grade prefix (smooth-ivory, digital-natural); generic words renamed (custom → custom-color/custom-foil, thermal → thermography/thermal-inkjet, corrugated → corrugated-roll/corrugated-sheet). **Total impact:** 595 leaves and groups receive new format notation, ~80 categories/sub-categories receive new hierarchical notation, 20 file renames, 0 orphan references, 0 collisions. Tools added: `tools/check_orphans.py` (URI integrity), `tools/check_notation_collisions.py` (uniqueness), `tools/generate_notations.py` (notation generator with dry-run/apply modes).

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/context`

### Structure: Product Model v1.0 — Sections-based Templates, full notation prefix migration, brochures consolidation

The Layer 1 Product Model reaches v1.0 with a comprehensive consolidation and naming overhaul, documented in the new standalone specification `PTPGE-Product-Model.md`. **Sections replace surfaces:** `perSurfaceSupports*` predicates are renamed to `perSectionSupports*` to align with the canonical "1 Section = 1 substrate-side = 1 Print Layer Stack" rule established in SD-0019. **Brochures consolidated:** ~138 deeply enumerated brochure leaf concepts (each format × orientation × fold-direction combination as a separate term) are replaced by **13 Brochure Templates**, one per Setup Model — single-fold, tri-fold, z-fold, gate-fold and its open/closed/double-closed variants, accordion-fold, tri-fold-stand, double-parallel-fold, cross-fold, cross-tri-fold, and roll-fold. All format, orientation, and fold-direction variation is now expressed via `supports*` predicates and resolved at Layer 2 (Configurator). **Setup Models expanded:** folded-sheet family grows from 1 pilot variant to **13 variants** covering the full topology space; `single-fold-4-panel` is renamed to `single-fold` to match the corresponding Folding term. **Controlled vocabularies introduced:** `/v1/ordering/use-cases/` (10 terms) and `/v1/ordering/audiences/` (7 terms), referenced from Product Templates via `typicalUse` and `typicalAudience` (now URI references rather than string literals). **Notation prefix migration:** all notations now carry top-concept prefix — `OP-` for Ordering Process, `PP-` for Printing Process — providing globally unique identifiers. Ordering Process top-concept gets notation `OP-O`; Printing Process gets `PP-P`. **Printing Process sub-category normalisation:** 27 sub-categories normalised to the 4-char length convention (e.g., `FD`→`PP-FOLD`, `QU`→`PP-QUAL`, `HG`→`PP-HYGR`). Two critical fixes addressed: `PS` duplicate (paper-suppliers / paper-surfaces) disambiguated to `PP-PASU` and `PP-PASF`; `PP` (printing-presses) renamed to `PP-PRPS` to avoid collision with the new PP- prefix. **Business Cards cleanup:** legacy `standards/color-systems/*` URIs migrated to `features/colors/*`; `features/surface-finishes/*` migrated to `completions/finishing/laminations/*`; technology URIs upgraded to current hierarchy; certification URIs corrected to `/environmental/` subtree; compound color concepts (`cmyk-plus-white`) replaced with atomic Layer-1-correct values (`white`); Business Card notation changes from `BCRD` to `OP-CARD-BUSI` following the 3-part product notation convention. **Total impact:** 480+ notations updated, 138 deep brochure leaves removed, 22 new vocabulary terms added (use-cases + audiences + setup models + brochure templates), zero deprecated URIs (this is pre-launch v1.0 — no breaking-change announcement required, but future releases will follow the standard deprecation path).

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/context`
- `https://ptpge.org/v1/ordering/setup-models`
- `https://ptpge.org/v1/ordering/setup-models/folded-sheet/single-fold`
- `https://ptpge.org/v1/ordering/use-cases`
- `https://ptpge.org/v1/ordering/audiences`
- `https://ptpge.org/v1/ordering/products/buy-online/sheets/brochures`
- `https://ptpge.org/v1/ordering/products/buy-online/sheets/business-cards`

## 2026-05-19

### Structure: Ordering Process Products: Hybrid Composition model + Setup Composition Grammar (SD-0019)

Ordering Process Products now use a **Hybrid Composition model**: Product Templates carry per-term `supports*` cross-references into Printing Process (instead of duplicating production vocabulary), plus a small set of product-conceptual `typical*` properties. A new **Setup Composition Grammar** describes the structural topology of each printed product via `ptpge:setupModel`, which references one of three setup families: *Flat Sheet* (2 surfaces — Front, Back), *Folded Sheet* (4–12 panels grouped in *outside* / *inside* macro-faces), and *Multi-page* (variable surface count grouped in *cover-outside* / *cover-inside* / *interior*). New URI hierarchy under `/v1/ordering/setup-models/`. New `ptpge:` predicates registered in `/v1/context`: `setupModel`, `surfaceCount`, `setupConfigurable`, `perSurfaceProperties`, `surfaceRoles`, `macroFaces`, `surfaceGroups`; full `supports*` inventory (Format, Orientation, Technology, Substrate, ColorMode, Embellishment, Folding, Binding, Certification, etc.); per-surface variants (`perSurfaceSupportsColorMode`, `perSurfaceSupportsEmbellishment`, etc.). Rule of thumb for the Layer 1 / Layer 2 boundary: per-term declarative constraints (*Brochure supports A4*) live in open Layer 1; cross-term combinatorial rules (*saddle-stitch requires page count divisible by 4*) live in licensed Layer 2. **Pilot scope of this release:** 2 Product Templates migrated to the new model (Business Card under `/v1/ordering/products/buy-online/sheets/business-cards`, and Single Fold A5 Portrait Left leaflet under `/v1/ordering/products/buy-online/sheets/brochures/single-fold/a5/portrait/left`); 2 of 3 Setup Model families published (Flat Sheet, Folded Sheet — Multi-page next); 199 duplicated option-tree nodes pruned (colors/, paper/, quantity/, embellishments/, delivery/ subtrees under Products that duplicated Printing Process concepts). Total term count drops from 932 to 734 as a result. The remaining ~30 leaflet/brochure variant Templates will follow the same pattern in subsequent releases.

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/context`
- `https://ptpge.org/v1/ordering/setup-models`
- `https://ptpge.org/v1/ordering/setup-models/flat-sheet`
- `https://ptpge.org/v1/ordering/setup-models/folded-sheet/single-fold-4-panel`
- `https://ptpge.org/v1/ordering/products/buy-online/sheets/business-cards`
- `https://ptpge.org/v1/ordering/products/buy-online/sheets/brochures/single-fold/a5/portrait/left`

## 2026-05-18

### Data: Spanish (`es`) and Simplified Chinese (`zh-Hans`) translations added across all 464 Printing Process terms

PTPGE Layer 1 now serves **six languages**: three at production quality (`en`, `nb`, `de` — native-speaker / professional review) plus Japanese, Spanish and Simplified Chinese as LLM-draft pending professional print-domain translator review. Coverage is complete on all 464 Printing Process terms — label, definition, and scopeNote where applicable. **Translation provenance is machine-readable.** The concept scheme at `/v1/scheme` now carries two new fields: `ptpge:translationStatus` (an object mapping each language code to either `"production"` or `"llm-draft"`) and `ptpge:translationProvenance` (detailed metadata about generation method and review status for non-production languages). API consumers that only want professional-reviewed content can filter on `translationStatus == "production"`. The three LLM-draft languages are part of PTPGE's published roadmap toward 14+ languages by end of 2027 via Founding Partner language sponsorship; professional translator review is scheduled for v1.1 (Q3 2026 — Japanese, highest priority because Japan is the global centre of digital production presses: Komori, Konica Minolta, Canon, Ricoh, Fujifilm, Mimaki, Mutoh, Roland DG, Brother, Mitsubishi, Horizon, Duplo), v1.2 (Q4 2026 — Spanish), and v1.3 (Q1 2027 — Simplified Chinese). The Ordering Process tree remains label-only for now and will receive multilingual coverage in a future release.

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/printing`

### Structure: v3 release: Ordering Process top concept added · Manufacturing renamed to Printing Process

Two structural changes ship together in this release. **First:** the original `Manufacturing` top concept is renamed to `Printing Process` (SD-0013). The new label is more precise (the standard describes the printing process, not generic manufacturing) and aligns vocabulary with how the industry actually talks. **All URIs under `/v1/manufacturing/...` have moved to `/v1/printing/...`** — this is a breaking change, but PTPGE is pre-launch and there are no public clients depending on the old paths yet. **Second:** the `Ordering Process` top concept ships in full (it was previously deferred). Ordering covers the end-user view — Knowledge (20 terms), Geography (38 terms), and Products (409 terms) — and lets the same vocabulary serve both manufacturing and end-customer ordering workflows (the two-role model). Combined, the API now exposes **932 terms** across **2 top concepts**, **10 Groups**, 35 categories, 148 sub-categories, and 737 leaf elements. The scheme.jsonld now lists both top concepts; the term schema's `group` enum is extended with the three Ordering groups; the per-term URI structure is unchanged within each branch.

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/printing`
- `https://ptpge.org/v1/ordering`

## 2026-05-08

### Branding: Layer 1 rebranded: PTPGE Taxonomy → **PTPGE Standard**

Layer 1 of PTPGE Print Infrastructure is now branded as **PTPGE Standard** (previously: PTPGE Taxonomy). The technical type description in API docs and RDF context is "a structured vocabulary". The tagline is "The open reference for print". Layer 2's role descriptor shifts from *Standard* to *Logic* (since "Standard" now lives at Layer 1). **No URIs change. No JSON-LD structure changes. No notation codes change. No API contract changes.** Public-facing changes: dashboard and editor titles, API landing-page subtitle, scheme.jsonld definition, README/STATUS/CONTRIBUTING/api-v1-README.md, glossary, decisions, layers, and skos-mapping pages. Rationale: with multilingual labels (4 languages), structured properties (width / height / unit / aspectRatio / standard / area), and external mappings (exactMatch / closeMatch), "Taxonomy" underdescribes what Layer 1 actually is. "Standard" positions PTPGE in the same category as ICC, ISO, JDF, and PDF/X — and differentiates clearly from UPT (Universal Product Taxonomy).

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/manufacturing`

### Data: Japanese (`ja`) language complete across all 464 terms

Japanese `definition` (and `scopeNote` where applicable) is now populated on every term in the Manufacturing branch — scheme, top concept, all 7 group `_index` files, and every leaf element across Standards (68), Management (51), Technologies (98), Configurations (118), Features (16), Substrates (74), and Completions (38). 464 of 464 terms (100%) now carry English (`en`), Norwegian Bokmål (`nb`), German (`de`), and Japanese (`ja`) content. Translations follow established Japanese print-industry terminology (印刷, 製版, 規格, 色空間, 色域, 認証制度, 用紙寸法, 製本, 仕上げ, 折り, 校正, etc.) and use loanwords (デジタル, インクジェット, ラミネート) where these are the established industry term. Strategic intent: Japan is the centre of gravity for the global digital production-press industry (Komori, Konica Minolta, Canon, Ricoh, Fujifilm, Mimaki, Mutoh, Roland DG, Brother, Mitsubishi, Horizon, Duplo) — having JA available removes a frequent adoption barrier for Japanese vendors and buyers.

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/manufacturing`

### Data: Japanese (`ja`) language added — Standards group (68 terms)

First Japanese batch — Standards group only. (Superseded by full-coverage release the same day.)

- `https://ptpge.org/v1/manufacturing/standards`

## 2026-05-07

### Data: German (`de`) language complete across all 464 terms

German `definition` (and `scopeNote` where applicable) is now populated on every term in the Manufacturing branch — scheme, top concept, all 7 group `_index` files, and every leaf element across Standards (68), Management (51), Technologies (98), Configurations (118), Features (16), Substrates (74), and Completions (38). 464 of 464 terms (100%) now carry English (`en`), Norwegian Bokmål (`nb`), and German (`de`) content. Translations follow standard German print-industry terminology (Bedruckstoff, Klebebindung, Drahtheftung, Wickelfalz, Bogenoffsetdruck, Tiefdruck, Stichtiefdruck, Briefumschlag, Visitenkarte, etc.).

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/manufacturing`

## 2026-05-06

### Data: Label typos fixed on A1 and BC/JP

Two Format-element labels carried typos that broke programmatic parsing of dimensions: `A1 (594m x 841 mm)` → `A1 (594 x 841 mm)`, and `Business Card Japan 91 x 55 mm)` (missing opening parenthesis) → `Business Card Japan (91 x 55 mm)`. Term URIs unchanged. Properties (width, height, etc.) populated unchanged.

- `https://ptpge.org/v1/manufacturing/configurations/formats/a-series/a1`
- `https://ptpge.org/v1/manufacturing/configurations/formats/business-cards/business-card-japan-91-x-55-mm`

### Schema: Pixel (`px`) added as valid `unit` value

The `unit` field on Format properties now accepts `mm`, `cm`, `in`, or `px`. Pixel-based formats — digital photo sizes, web banners, social-media formats, screen resolutions — are first-class Layer 1 formats alongside paper. Pixel-based `area` is in `px²`; `aspectRatio` is dimensionless and remains comparable across all unit types.

### Schema: Field renamed: `attributes` → `properties`

The element-level descriptive-data field is now called `properties` on every term, in the JSON-LD context (predicate `ptpge:hasProperty`), and in the schema. The previous label `attributes` was overloaded with the editor's UI section header; renaming aligns with Schema.org / RDF / eCl@ss convention. **Breaking change in shape** — clients that read the `attributes` key must switch to `properties`.

## 2026-05-05

### Data: Format properties populated on 101 of 102 leaf elements

Every rectangular Format element in `configurations/formats/` now carries structured `properties`: `width`, `height`, `unit`, `aspectRatio`, `standard`, `area`. A4 receives `{width: 210, height: 297, unit: "mm", aspectRatio: 1.4142, standard: "ISO 216", area: 62370}` and equivalents for all other formats. The single exception is `customized` (X), which intentionally has no fixed dimensions.

- `https://ptpge.org/v1/manufacturing/configurations/formats`

## 2026-05-03

### Data: Initial Manufacturing release — version 2026-05-03

First publicly-tracked release of the PTPGE Standard. 464 terms across the seven Manufacturing groups: Standards (68), Management (51), Technologies (98), Configurations (119), Features (16), Substrates (75), Completions (36). 1 top concept, 7 groups, 22 categories, 54 sub-categories, 380 elements. Every term has a permanent URI under `https://ptpge.org/v1/manufacturing/...`. English and Norwegian Bokmål definitions populated. Cross-walks to Wikidata and other vocabularies via `exactMatch` / `closeMatch`.

- `https://ptpge.org/v1/scheme`
- `https://ptpge.org/v1/manufacturing`
