# Data model

The working database (the **master**) is one editable Excel workbook with 27 sheets: 21 data tables and 6 reference sheets. It has 311 fields and 379 codes. The full lists are in [`../schema/`](../schema/).

## 1. Tables

| Module | Table | Excel sheet | One row is | Primary key |
|---|---|---|---|---|
| Source and time | `SOURCES` | ДЖЕРЕЛА | One specific version of a document or file | `source_id` |
| | `ENTRIES` | ОПИСИ | One numbered entry in a section of the source | `entry_id` |
| | `LAYERS` | ШАРИ | One original record, addition or correction of an entry | `layer_id` |
| | `EVIDENCE` | ДЖЕРЕЛЬНІ_ТЕКСТИ | One located passage of a specific version of the source | `evidence_id` |
| People and co-residence | `ACTORS` | РЕЄСТР_УЧАСНИКІВ | One participant (person, institution, named collective) | `actor_id` |
| | `PERSON_FACTS` | ОСОБИ | The characteristics of one person in one layer | `person_fact_id` |
| | `GROUPS` | ГРУПИ | One group of a defined type and scope | `group_id` |
| | `MEMBERSHIPS` | СКЛАД_ГРУП | One membership of a participant in a group in one layer | `membership_id` |
| | `RELATIONS` | ЗВЯЗКИ | One directed statement of a relation between two participants | `relation_id` |
| Property and economy | `ASSETS` | МАЙНО | One property object, or an explicitly collective set of objects | `asset_id` |
| | `ASSET_FACTS` | КІЛЬКОСТІ_ТА_СТАН | One measurement or characteristic of an object in one layer | `asset_fact_id` |
| | `ECONOMIC` | ЕКОНОМІКА | One economic formula, or one component of a composite statement | `economic_id` |
| | `PARTICIPANTS` | СТОРОНИ | One party (participant or group) to an economic statement | `participant_id` |
| Health and events | `HEALTH_OBS` | ЗДОРОВЯ | One condition or functional limitation of a person | `health_id` |
| | `LIFE_EVENTS` | ПОДІЇ | One directly reported event | `event_id` |
| Dictionaries and control | `PLACES` | ТОПОНІМИ | One place in its historical context | `place_id` |
| | `TERMS` | ТЕРМІНИ | One versioned decision on a term in a defined context | `term_id` |
| | `COVERAGE` | ПОВНОТА | One information domain of an entry in one layer | `coverage_id` |
| | `REVIEW` | СПІРНІ_РІШЕННЯ | One question, or one change of a specific record | `review_id` |
| | `TRACE` | ПІДСТАВИ | One combination of record, evidence and import origin | `trace_id` |
| | `CHANGE_NOTES` | ПОЯСНЕННЯ_ПРАВОК | One explanation of a manual correction | `change_id` |

Reference sheets: **ПОЧАТОК** (how to start), **СТРУКТУРА** (the table list above), **ПРАВИЛА** (principles F01–F18), **КОНТРОЛЬ** (row counts and rows with problems), **ПОЛЯ** (the field dictionary), **КОДИ** (the code lists).

## 2. Main relations

- `ENTRIES` 1—N `LAYERS`. The original record has the suffix `_I`; later layers `_S`, `_S1`, `_S2`, `_M`, `_D`.
- `LAYERS` 1—N `PERSON_FACTS`. A profile links to `ACTORS` and to its layer. One participant may have several profiles: one per layer, and one per entry after persons were merged across entries.
- `GROUPS` (within an entry and a layer, nested by `parent_group_id`) 1—N `MEMBERSHIPS` N—1 `ACTORS`.
- `RELATIONS`: directed pairs `from_actor_id` → `to_actor_id` within a layer, with a basis and a link status (`accepted`, `candidate`, `rejected`).
- `ASSETS` nested from complex to component by `parent_asset_id`; `ASSET_FACTS` holds counts, condition and use by layer.
- `ECONOMIC` statements within a layer, linked to an asset, a term and a parent statement. `PARTICIPANTS` link each statement to an actor **or** a group, never both.
- `HEALTH_OBS` and `LIFE_EVENTS` are linked to a participant and a layer.
- `PLACES` are nested (a village within a regiment). They are referenced from entries, persons (origin) and assets.
- Every substantive row points to `EVIDENCE`. `TRACE` records the basis of groups of fields; `COVERAGE` records the completeness of each information domain.

## 3. Keys

- **Opaque, stable IDs.** Each table has its own prefix (`ACT_`, `PER_`, `GRO_`, `MEM_`, `REL_`, `ASS_`, `ASSF_`, `ECO_`, `PAR_`, `HEA_`, `LIF_`, `PLA_`, `EVI_`, `COV_`, `REV_`, `TRACE_`) and a zero-padded counter. Log tables may also carry block prefixes. IDs are never renumbered; each table has its own counter. *(C1)*
- **Entry IDs** are `CITY_nnn` and `FOR_nnn`. The printed number is a separate field. *(B3)*
- **Layer IDs** are the entry ID plus a suffix: `CITY_176_I`, `FOR_467_S`.
- **Settlement prefix.** In a release every key carries the settlement code (`POL_PER_00001`, `POL_CITY_012`) and every table has `settlement_id`.

## 4. Field modes

Each field in the dictionary has a mode:

- **edit** — a primary fact, entered and corrected by hand.
- **derived** — a reference formula from related tables, shown in grey with "(auto)" in the header. Edit the source table, not this column. Derived columns are not primary facts when read into R.

The workbook also has a row-control column on each data sheet, highlighting of duplicate IDs, input validation for codes and foreign keys, and a control sheet with counts of empty IDs and impossible age bounds. These help manual editing; they are not a full QA (see [`quality-assurance.md`](quality-assurance.md)).

## 5. Why Excel

The master is used by hand: opened, filtered, cleaned, corrected and then read into R. Excel was kept deliberately instead of a database server. The price is that Excel keeps no history of manual edits, so substantive corrections go through `CHANGE_NOTES`, and every release is checked by script after export.

## 6. The public release

A release for users is built from the master and differs from it:

- **17 tables**: the data tables without `EVIDENCE` texts, `REVIEW`, `COVERAGE`, `TRACE` and `CHANGE_NOTES`. Evidence is replaced by `source_refs`, which holds locators only (printed page, PDF page, folio, segment, transcription status).
- **No source texts.** The edition is not reproduced.
- **Two complete versions**: Ukrainian (Ukrainian field names and code labels) and English (English field names and codes, names transliterated by the KMU 2010 system, a glossary of historical terms).
- **For each version**: one Excel file with filters and no formulas, one UTF-8 CSV per table, a data dictionary and an R loader.
- **A derived residents table** for demographic counts, with its own inclusion rules ([`residents.md`](residents.md)).

## 7. Combining settlements

- Every ID carries the settlement prefix and every table has `settlement_id`, so tables of different settlements coded with the same code lists can be stacked (`rbind`) without key conflicts.
- A new settlement needs its own code, the same code lists and, for any section that is not town or suburb, an agreed value of `section` (or a documented extension of the list).
- Volumes for other settlements may record things Poltava does not (livestock, fields measured in sazhens, forest, bees). Test them against the existing fields first; add fields only as a versioned change of the dictionary. *(F18)*
- Do not identify persons across settlements in the core tables. Build a separate table of candidate links.
