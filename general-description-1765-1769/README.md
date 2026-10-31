# General Description of Little Russia, 1765–1769: coding method

**Version 1.0 · 31 October 2026 · Igor Serdiuk**

The General Description of Little Russia (*Heneralnyi opys Malorosii*), also known as the Rumiantsev Description, is an inventory of the Hetmanate compiled in 1765–1769. For each yard it records who lived there, their ages and kin relations, workers and their pay, buildings, land, trades, dependencies and much else. It is one of the richest sources for the social, demographic and economic history of eighteenth-century Ukraine.

This folder documents a method for coding the Description into a relational research database. The method was developed and tested on the description of **Poltava**. It is designed to be applied to other settlements without changes to the core structure, so that their databases can be merged.

The method is built around one requirement: **an empty place in the database must never be mistaken for silence of the source, and silence of the source must never be mistaken for absence**. Every value carries a status, every statement is tied to a chronological layer and to a locator in the edition, and every interpretive step is either a documented rule or a logged decision.

## Status

- **Poltava fully coded.** Town (fortress) entries 1–256, suburb (*forshtat*) entries 1–911 and one unnumbered entry. Original records date from 1 November 1765 to 3 February 1766 (Julian calendar); later additions reach 1768–1769.
- **Method version 1.0** corresponds to internal master version 18.
- **Dataset.** The Poltava database will be released separately, in Ukrainian and English versions with a codebook and a DOI. It is not part of this repository.

## What is in this folder

| Path | Contents |
|---|---|
| [`docs/coding-manual.md`](docs/coding-manual.md) | The coding rules: principles, standard rules and worked decisions, grouped by topic |
| [`docs/data-model.md`](docs/data-model.md) | Tables, units of analysis, keys, layers, the Excel master, the public release, combining settlements |
| [`docs/source-reading.md`](docs/source-reading.md) | Working with the edition: witnesses, locators, reading conventions, evidence texts |
| [`docs/quality-assurance.md`](docs/quality-assurance.md) | Structural checks, plausibility checks, independent source audits, release protocol |
| [`docs/workflow.md`](docs/workflow.md) | How the coding was organised: blocks, decision log, AI coding agents, hand-over packages, lessons learned |
| [`docs/residents.md`](docs/residents.md) | Rules for deriving a table of residents for demographic analysis |
| [`schema/tables.csv`](schema/tables.csv) | The 21 data tables of the master |
| [`schema/fields.csv`](schema/fields.csv) | Every field: type, required, code list, foreign key, edit mode, description (English, with the Ukrainian Excel headers) |
| [`schema/codes.csv`](schema/codes.csv) | Every code list and code, with Ukrainian and English labels |
| [`tests/test-scenarios.yaml`](tests/test-scenarios.yaml) | Synthetic situations with the expected coding, for checking a coder before real work |

## Core principles in brief

1. **Units are kept apart.** An entry of the source, a yard, an enumerated residential group, a family and a household are different units. None is equated with another by default; households are not reconstructed without an explicit rule.
2. **A value is separate from its status.** Each characteristic has a status: recorded, explicit zero, explicit absence, informant did not know, deferred, not recorded, illegible, conflicting, scope uncertain, not assessed, not applicable. An unknown age is never zero; an unstated wage is never zero.
3. **Time is layered.** The original record is never overwritten by a later addition, correction or marginal note. Each layer has its own profiles of people, and later profiles hold only what that layer says.
4. **People are not invented.** A specific person whose name is omitted is created as an unnamed person; an unnamed group of unknown size ("with wife and children") is never turned into individuals.
5. **Characteristics are not inherited.** The estate, occupation and health of the head do not pass to relatives. Being named in an entry does not make someone a resident.
6. **Identity is rule-based.** Merging persons across entries follows explicit rules; everything else stays a candidate link.
7. **Everything is traceable.** Each statement points to its evidence (printed page, PDF page, manuscript folio as given by the edition) and to the rule or decision behind it.
8. **The master stays human.** The working database is an editable Excel workbook with stable IDs, validation and control columns, ready to be read into R.

## Applying the method to another settlement

1. Choose a settlement code (for example `PER` for Pereiaslav) and use it as the prefix of every ID and as `settlement_id` in every table.
2. Use the same tables, fields and code lists ([`schema/`](schema/)). If the source shows a phenomenon the code lists cannot express, keep the source text, log the problem and extend the code list as a documented, versioned change; never repurpose an existing code.
3. Code in blocks of about 25 entries, following [`docs/coding-manual.md`](docs/coding-manual.md) and recording every new decision in your own decision log.
4. Run the checks in [`docs/quality-assurance.md`](docs/quality-assurance.md) after every block and an independent source audit at least once per section.
5. Do not create cross-settlement identifications in the core tables; build them as a separate table of candidate links.

## Sources and copyright

The Poltava database was coded from the published edition of the Poltava description, edited by Yurii Voloshyn (Kyiv: Nash Chas; the PDF copy used gives the year 2010). The edition and its transcriptions are not reproduced here. The method documents refer to entries by number (for example *FOR 98*, *CITY 176*) so that each decision can be traced; short source formulas are quoted only where a rule depends on their wording. The test scenarios use synthetic examples.

## How to cite

Serdiuk, Igor. *General Description of Little Russia, 1765–1769: coding method*, version 1.0. In: *Working with Sources*. GitHub, 2026. https://github.com/Serdiukan/working-with-sources

See also [`CITATION.cff`](../CITATION.cff) at the root of the repository.

## License

[CC BY 4.0](../LICENSE). You may reuse and adapt the method with attribution.
