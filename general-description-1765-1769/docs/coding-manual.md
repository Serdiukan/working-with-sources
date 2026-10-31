# Coding manual

This manual sets out the rules used to code the General Description of the Hetmanate (1765–1769) for Poltava. Field names and codes refer to [`../schema/fields.csv`](../schema/fields.csv) and [`../schema/codes.csv`](../schema/codes.csv).

## 0. How the rules are organised

The rules come from four layers, each with its own IDs:

| IDs | Layer | Origin |
|---|---|---|
| F01–F18 | **Principles** of the core structure (version 1.0) | Fixed with the data model |
| A1–A17 | **Standard rules** | Set or approved by the researcher |
| B1–B48 | **Decisions** made during coding, usually on a concrete case | Logged with block, date, source basis and what they supersede |
| C1–C4 | **Technical rules** of the builder | Do not change the meaning of the source |

**Precedence.** A direct decision of the researcher overrides the specification, and the specification overrides the current fields and codes. A contradiction found between them is logged, not silently resolved.

**Rules are never deleted.** A rule that changes is marked *superseded by …*, and the old one stays in the log. In this manual the rules are grouped by topic; the IDs in brackets point to the decision log of the Poltava run, so each rule can be traced to the case that produced it.

Entries are cited as `CITY n` (town) and `FOR n` (suburb).

---

## 1. Source and witnesses

1. **One witness, explicitly chosen.** In the Poltava run the only witness is the supplied PDF of the edition, identified by the SHA-256 of each file. Every reading rests on its text layer checked against the page image at 200 dpi. *(A17)*
2. **Other versions are not silent witnesses.** The editor's Word transcription was excluded as a witness by the researcher's decision: no age, name, sum or status rests on it. Its row is kept in the source register only to keep references intact. This supersedes the part of F05 that treated Word and PDF as parallel witnesses. For a new settlement, decide this explicitly before coding begins. *(F05, A17)*
3. **Earlier databases give candidates only.** Values imported from spreadsheet extracts are checked against the witness before use; their role is `import_candidate`. *(F05)*
4. **The manuscript was not consulted.** "Checked against the source" means checked against the edition. Never write "checked against the original" unless the manuscript was actually read.
5. **Editorial notes of the edition are evidence, not text.** Footnotes and editorial marginal notes (corrections, conjectures, *sic*, "other hand", "darker ink") help to separate layers. They never replace the text of the document and never create persons. *(B13, B32)*
6. **Editorial conjectures.** A note such as "evidently a slip; should be Yakiv" does not change the coding. Code by the text; record the conjecture as a candidate identification in the participant's note and in the review log, status *resolved: by the text of the source; editorial conjecture is a candidate*. *(B32)*

Reading conventions, locators and evidence texts are in [`source-reading.md`](source-reading.md).

## 2. Entries and sections

1. **The entry is the unit of description.** One numbered description of the source is one entry. It is not necessarily a yard and never automatically a household. *(F01, F02)*
2. **Sections are kept apart.** Town (fortress) and suburb are separate sections with their own entry numbering (`CITY_…`, `FOR_…`). They must be analysable separately and combinable. The spatial meaning of a section is not assumed.
3. **IDs are not entry numbers.** The ID comes from the table counter; the number printed in the source is kept in `source_number_raw` and `source_number`, and the position in the source sequence in `source_order`. *(B3, C1)*
4. **An unnumbered record in the main hand** between numbered entries, describing a separate object, is a separate entry with `source_number_raw` = "without number (after 620)" and an empty numeric number. An unnumbered passage in another hand or ink is a later layer of the preceding entry. *(B29)*
5. **A record the source itself marks as an erroneous repeat:** both entries are kept, the repeat note becomes its own layer, the probable duplicate is logged in the review log for both entries, and nothing is counted twice. *(B16)*
6. **Crossed-out entries** are recorded in full and marked as crossed out (`layer_kind` / `enumeration` = `crossed_out`). They are excluded from demographic counts but stay visible for studies of registration and property.
7. **Documentation mode.** A general deferral ("he will show the rest to the commission") or an absent owner changes what the silence of an entry means. Record it in `documentation_mode` for the entry; affected values get `deferred` or `informant_unknown`, never zero.

## 3. Layers and dates

1. **The original record is never overwritten.** Each entry has an original layer (`_I`). Later additions, corrections, crossed-out text and passages in another hand or ink are separate layers (`_S`, `_S1`, `_S2`, `_M` marginal note, `_D` dated addition). *(F06)*
2. **Different hands are not merged.** Several later layers in one entry stay separate even if they look contemporary. *(B28)*
3. **Darker ink or another hand** gives grounds for a separate layer of kind `uncertain` with no date. Persons and facts known only from that fragment are not coded in the original layer; people already present get a sparse profile in the new layer. *(B15)*
4. **A superscript word without signs of another hand** is coded in the original layer with a note. *(B11)*
5. **Later profiles are sparse.** A later layer holds only what it says. If an age first appears in an addition, the original profile keeps the age as `not_recorded`; the number goes into the profile of the later layer. A correction does not restore an erased original value.
6. **Historical notes of the document create persons in their own layer.** A note that a daughter was born, or that someone left or died, creates the person and the event in that layer. *(B25)*
7. **Events from later notes have interval dates.** A birth reported in a later note is a `birth` event dated as an interval from the date of the entry to the date of the note; the day is never invented. The related participant is the father or the informant. *(B30)*
8. **Four kinds of date are kept apart:** the date of the record, the date of an addition or report, the date of the historical event and the term of a contract or payment. *(F06)*
9. **Day headings.** The date of a day heading is applied to the following entries only after the sequence of headings has been checked. Where the sequence shows that a printed month is wrong (in Poltava, "November 3" and "November 5" headings that fall in December), the date follows the sequence and the source form is kept in the date-source field. *(B37)*
10. **Dates are ISO intervals in the calendar of the source.** Partial dates get a start and end with a precision flag. The calendar is not converted; a converted date is only ever a separate derived variable. *(F06)*

## 4. Values and their status

Every characteristic has a value and a separate status (`value_status`). An empty cell means nothing by itself. *(F03)*

| Status | Use |
|---|---|
| `recorded_positive` | The fact or number is recorded. This says nothing about its precision. |
| `explicit_zero` | A zero is explicitly stated. Only then is the numeric value 0. |
| `explicit_absence` | The absence of a specific thing is explicitly stated. |
| `not_recorded` | No formula. Silence of the source is not absence. |
| `informant_unknown` | The informant explicitly did not know. |
| `deferred` | The information is promised or deferred ("will show to the commission"). Not zero, not absence. |
| `illegible` | The text cannot be read. |
| `conflicting` | Witnesses or readings conflict. No single number is given; alternatives are never averaged or turned into an interval. *(A9)* |
| `scope_uncertain` | A formula exists, but it may not apply to this person or object. |
| `unassessed` | Not yet checked. |
| `not_applicable` | The characteristic does not apply. |

Further rules:

- An unknown age is an empty number with a status, never 0.
- An unstated wage is never 0; "on maintenance" (*на пропитание*) is not a zero wage.
- "He will show the rest" means neither absence of property nor ignorance of the informant: it is `deferred`.
- Numbers keep their qualifier: `exact`, `approximate`, `upper_bound` ("up to 300"), `lower_bound`, `interval`.

## 5. Names

1. **The source form is always kept** (`*_raw_source_form`). Normalised forms are a separate field.
2. **The edition's name index gives the normal form** when the surname matches, the given name is similar and the page agrees. In the Poltava run the similarity threshold was 0.75. *(A1)*
3. **Below the threshold** the name is written as in the text and the variant goes to the review log (e.g. *Avdotia / Yevdokiia*). *(B6)*
4. **Church and vernacular forms of one name are one name**, normalised to the standard form: *Afanasii / Opanas*, *Ioakym / Yakym*, *Katria / Kateryna*, *Vanka, Ivashka / Ivan*. The similarity threshold does not apply to such pairs. *(B9)*
5. **Surname variants** of the same person (*Solenyk / Solenychenko*) get a main form, with the variant in a note. The researcher's chosen spelling is used consistently across entries. *(A3)*
6. **Heading versus signature.** Where the form in the heading or first mention (as owner or head) differs from the signature, the first mention is the main form and the signature form is a variant. This also applies to text written over an erasure. *(A11)*
7. **Parsing the name index.** The index of the edition is parsed by coordinates (two columns, joined hyphenations and continuation lines), not by text lines. *(B43)*

## 6. People and identity

### Who is a person

1. **`ACTORS` holds people, institutions and named collectives; `PERSON_FACTS` holds only people**, one profile per person and layer. A profile is an observation, not a person; profile counts are not population counts. *(F07)*
2. **Two people with the same name in one entry** but of different age or relation are two persons. *(A5)* The same name and the same age with no other distinction are also two persons, with a flag (*FOR 98*: two sons with the same name, both aged eleven). *(B4)*
3. **Specific unnamed persons are created.** If the source attests one concrete person whose name is omitted ("daughter … years old", "uncle … years old"), create an unnamed person (`identification` = `unnamed_individual`, normalised name "… (name not given)") with membership and relations. *(A6 as refined)*
4. **Unnamed groups of unknown size are never turned into individuals.** "With wife and children" gives a separate unnamed wife, but no invented number of children; the children are an `unnamed_collective` and the entry is flagged for analysis. *(A6)*
5. **A hiring formula with no named worker** (a scribe's omission) does not create a person. *(B24)*
6. **Being named is not being resident.** An owner, informant, signer, seller, previous owner or deceased relative named in an entry is recorded with a mention role and `enumeration` = `mentioned_only`, and is excluded from demographic counts by default. No new fields or ID sets are added for them. *(F08)*

### Merging across entries

Merging makes one participant out of records in several entries. It follows explicit rules only:

| Rule | Merged | ID |
|---|---|---|
| Signers | A signer (also a witness signing as "the undersigned") with the same given name and surname in different entries is one person. Variants that differ by one or two letters, or by the unification rules (і/ѣ/е, и/і, final к/т), count as the same surname. Church and vernacular forms of the given name count as the same name. | A10, B9, B12 |
| Signing residents | A resident son who signed for his father is merged with a signer of the same name. | B10 |
| Name plus rank or occupation | The same full name with the same rank, office or occupation in different entries is one person, even if not a signer. | A12 |
| Craftsmen | A craftsman of the same town with the same name and occupation is one person. | A2 |
| Occupational surnames | A surname derived from an occupation (*Kotliar / Kotliarenko*) plus the same occupation and name in neighbouring entries is grounds for merging. | A12a |
| Titles | A unique title (e.g. *count*) with the same name. | A12b |
| Wording of service | Different wordings of service in one institution (clerk / standard-bearer fellow of the general artillery) do not prevent merging; each wording stays in the profile of its layer. | A12c |
| Derived surname in one entry | A derived surname (*Artiukh → Artiushenko*) of the same name and rank in the same entry. | A12d |

Not merged:

- **Different surnames** that are not variants (*Pavlenko ≠ Pashchenko*, *Artemenko ≠ Artiushenko*). *(B12, B14)*
- **Owners or informants** merely because they share a name with a signer. *(B12)*
- **Residents who did not sign**, by the signer rule. *(A10)*
- **Doubtful cases** (no rank, different ranks, several namesakes in one entry). These become candidate links: `relation_type` = `same_entity`, `link_status` = `candidate`. A coincidence of names alone is never enough. *(A12, F07)*

The canonical ID of a merged person is the one with most signing links, or the form from the name index. Identification links that became loops after a merge are deleted. *(B12, A12)*

## 7. Residence and groups

1. **Ownership, residence, physical presence and being listed are four different characteristics** (`economic` ownership, `residence_code`, `presence_code`, `enumeration_code`). *(F08)*
2. **Residence codes:** `explicit_resident`, `contextual_resident` (listed among the people of the yard), `explicit_nonresident`, `not_established`, `disputed`.
3. **Groups.** The root group of an entry is the enumerated residential group (yard). Nested groups are huts and families. An enumerated residential group is not a household. *(F02)*
4. **Families** (`group_type` = `family`) are reconstructed from direct kinship formulas only: a connected set of accepted kin links among the listed residents of the original layer becomes a family group inside the smallest enumerated group that contains it (also when the two coincide). Roles and residence come from the enumerated group; positions follow the source order. *(B46)*
5. **Households are not reconstructed** in this version. A household needs its own rule and evidence. *(F02)*
6. **Source order.** `source_person_order` follows the text, not the order in which records were created. One person in several groups (yard, hut, family) is one person.
7. **A lodger living without pay** is a lodger in the group; no rent statement is created. *(B19)*

## 8. Kinship

1. **Pair links are stored independently of groups** (`RELATIONS`). Parenthood, motherhood and marriage are never inferred from age or position in the list alone. *(F09)*
2. **Pronoun formulas** ("he / they have workers, children, a son-in-law…") refer to the nearest named person or couple. A chain of identical pronouns ("he has a son … he has workers") refers to the same antecedent; ambiguous cases stay open in the review log. For the employer in hiring formulas, section 10.3 takes precedence. *(B36, B40)*
3. **Plural and singular:**
   - "they have children / a son / a daughter" gives `father_of` and `mother_of` for both spouses;
   - "he has a son" gives the father only; the motherhood of the present wife is not assumed. *(B47)*
4. **Implausible age gaps.** A parent–child link with a gap under 12 or over 55 years is only a candidate, with a note. *(B47, B48)*
5. **Order of a family in the record.** The eldest son with his wife and children comes first, then the other children of the head. Children listed after the son's family with no new pronoun are the head's children. *(A13)*
6. **"Y, son-in-law of X" headings** (e.g. "Trofym, Liakh's son-in-law"): the householder is Y, and he is X's son-in-law; his own surname is not established. X is only mentioned and is not created as a person. *(A15)*
7. **Historical kin terms** (*ziat*, *svoiak*, *pryiemysh*, *teshcha*) are read in context. Where the exact link is not proven, use `other_kin` and keep the source term in the note. A foster child is not legally adopted unless the source says so. *(B45)*
8. **A patronymic is not a parent.** "Ivan's daughter" does not create Ivan as a person.

## 9. Property

1. **Object, characteristic and right are separate records:** the object in `ASSETS`, counts and condition in `ASSET_FACTS`, the right as an `ownership` statement in `ECONOMIC` with an `owner` party. *(F10)*
2. **The heading formula "Yard / Pidvarok … of X" makes X the owner:** an `ownership` statement with `recorded_positive`, X as party with role `owner`. It does not apply to other headings (huts without a yard, an empty plot, an institution without the formula). Doubtful cases are listed and reviewed. *(A14)*
3. **Explicit text beats the heading formula.** In church yards, where the text says "in the possession / own", the cleric is the owner; otherwise the institution is the owner. *(B41)*
4. **Ownership does not make residence.** A non-resident owner is mentioned only.
5. **Complexes and components are never summed.** One of two huts is part of the two, not a third hut (`parent_asset_id`, `object_scope`). *(F10)*
6. **Room, hut, yard and plot are different types**; area, distance and quantity have their own units. *(F10)*

## 10. Hiring and money

1. **Every economic statement has a type and parties.** Hiring, wage, maintenance, rent, deal, capital, debt, dues, production and trade are separate types; parties refer either to an actor or to a group, never both. *(F11)*
2. **Components of hiring are coded separately:** cash wage, food (*пища хозяйская*), clothing (*платье хозяйское*), footwear, training in a craft, maintenance without wage (*на пропитание*). *(A8)*
   - "On his own clothes" (*на своем платье*) is a clothing statement with `explicit_absence`: the master does not provide clothing. It does not mean the worker has none. *(B40)*
   - Food or clothing not mentioned is not borrowed from a neighbouring hiring.
   - Training in a craft does not establish the master's occupation.
3. **Who pays.** The employer is the head of the yard unless the text names someone else. At entry level this is the resident owner; otherwise the head of the first hut of the root group; in a pidvarok with subjects, the head of the subject's family; if the owner does not live there, the head of the residents' family (not a worker's) that precedes the worker. Group employers, explicitly named non-residents and workers who are themselves heads are not changed by this rule. For hiring, this rule takes precedence over the pronoun rule of section 8.2. *(A16, A16a)*
4. **Sums:**
   - "For two rubles" for two hired people: each person gets two rubles (`per_each_named`). *(A7)*
   - An explicitly joint sum ("with his wife", "with the children") is a group amount (`shared_group`) and is never divided among persons. *(F11)*
   - "Without pay" (*беззаплаты*) is applied only to the scope the formula covers.
   - A repeated sum that is a scribe's slip is not counted twice. *(B21)*
   - "Living in the hut for N" (*живут из найму в хате за N*) is rent, a joint sum, not a wage. *(B40)*
5. **Units and periods.**
   - *Pogodno* means per year. Half-yearly and weekly pay are never replaced by a yearly figure. *(B36, F11)*
   - A unit named in a neighbouring sum of the same list is applied, with a note. *(B36)*
   - Unknown units are not converted. Kopecks are converted to rubles only with an explicit note; the source formula is kept.
   - No income is derived from the existence of a bathhouse, distillery or shop.

## 11. Health, literacy, estate and occupation

### Health

1. **Keep the formula, its scope and each manifestation.** No modern diagnosis, no inferred inability to work, no cause of death. *(F12)*
2. **A health formula applies to the list it closes** *(B35, B20, B45)*:
   - after the wife: to the couple;
   - after the children: to the children; the parents get `scope_uncertain`;
   - after another relative: to that relative; the others get `scope_uncertain`;
   - a singular formula: to the nearest person.
3. **A named condition overrides the general formula.** "Healthy, limps on the right leg" keeps both: the profile gets the condition category and the limp is a separate manifestation in `HEALTH_OBS` with body part and side.
4. **Age formulas do not belong in the health field.** *(B45)*
5. **Height** is recorded in arshins and vershoks as given; centimetres are only a derived value by a versioned rule. *(F12)*

### Literacy

6. **Signing for someone else is a link, not literacy.** "Instead of X, illiterate, Y signed" gives a `signs_for` link from Y to X and `explicit_illiterate` for X. It says nothing about Y's literacy beyond the signature. *(A4, B7)*
7. **"Signed himself"** is `signs_for_self`: a personal signature, which is not the same as full literacy. *(B7)*
8. **"Instead of X, at his request"** without the word "illiterate" leaves X's literacy as ambiguous / not recorded. *(B40)*
9. **Search all signing formulas**, including variants such as "not able to write" and "put his hand instead of …", across the full texts of the whole base. *(B47)*

### Estate, rank, office, occupation, dependency

10. **These are separate characteristics.** Social estate, rank, office, occupation, dependency, wealth and an analytical class (HISCLASS) are never merged into one field. *(F13)*
11. **No inheritance.** The estate, occupation and health of the head do not pass to relatives. A gunner's widow is not a gunner. A Cossack's son is `cossack_son`, not `cossack`.
12. **Dependency is not serfdom.** "Subject of an owner" (*подданый*) is `dependency` = `subject`; legal serfdom is coded only when explicitly stated.
13. **Occupations are not guessed.** English equivalents carry a mapping status. HISCO/HISCLASS codes are added only by a separate, verified mapping, never to raise the share of filled cells. *(F13)*

## 12. Places, terms and transliteration

1. **Source toponym, modern identification, coordinates and the location of a yard are different levels.** The point of a town is not the coordinate of a yard. Uncertain identifications stay candidates. *(F14)*
2. **Toponyms of different types are not merged**, even with the same name (a river and a town called *Holtva* stay apart); the same toponym of the same type recorded twice is merged. *(B38)*
3. **A route heading is a sequence of the source, not street geometry.**
4. **Terms** have a domain, an English equivalent in the historical sense and a mapping status (`proposed`, `context_required`, `verified`, `no_equivalent`, `disputed`). Historical categories are not translated by modern concepts with wider legal meaning.
5. **Transliteration** of names and toponyms follows the official Ukrainian system (Cabinet of Ministers Resolution No. 55, 2010). Letters outside the modern alphabet get the nearest value: ъ → —, ы → y, ѣ → i, ѳ → f, ѯ → ks, ѿ → ot. The Cyrillic form is always kept.

## 13. Coverage, review, trace and change notes

1. **Coverage** (`COVERAGE`) is assessed for each entry, layer and one of 18 information domains (people, kinship, residence, health, social characteristics, buildings, land, remote property, rights and documents, labour and pay, trade, production, capital, debts, taxes and dues, livestock, signatures and informants, anthropometry). It measures what the source records, not the share of filled cells. An addition does not change the assessment of the original layer. *(F15)*
2. **Review log** (`REVIEW`) holds open questions, explained decisions and irreducible variants. Statuses: `open`, `resolved`, `awaiting_pdf`, `awaiting_manuscript`, `irreducible`. A question is not marked resolved just because a convenient option was chosen.
3. **Questions to the researcher** are asked only for a genuinely unresolvable source or structural ambiguity, one at a time. A clear rule is applied without asking.
4. **Trace** (`TRACE`) records the basis of groups of fields: which evidence, which rule, which import. One record can cover several fields; exceptions and interpretations are explicit. Cell-by-cell provenance is not used.
5. **Change notes** (`CHANGE_NOTES`) explain every substantive correction of an existing row: table, ID, field, old and new value, reason, evidence. Excel keeps no history of manual edits, so this log is the history. *(F16, C2)*
6. **A phenomenon the schema cannot express** is kept as source text, logged as a limitation, and reported. No new entity is created to get round the schema. *(F18)*

## 14. Technical rules

1. **IDs are never renumbered.** The counter of a table is the maximum numeric suffix of its prefix in that table, not a counter shared by the whole corpus. *(C1)*
2. **Earlier rows are not changed** by new blocks. New rows are appended; corrections of old rows go through change notes. *(C2)*
3. **Control formulas** are copied from the last row of the sheet. *(C3)*
4. **A candidate file is not the master** until QA passes. A failed candidate never replaces the master. *(C4)*
5. **Formula ranges must reach beyond the data.** Absolute ranges in derived columns, conditional formatting and validation are extended after every block (to at least 1.5 × the rows of the target table, or rows + 2,000). *(B34)*
6. **Code values must belong to their code list.** *(B39)*
7. **The code lists change only by sanctioned decision.** In the Poltava run one code was added (`life_event` = `birth`) by the researcher's decision, and QA allows exactly that addition. *(B30, F18)*

## 15. Principles of the core structure

| ID | Principle |
|---|---|
| F01 | **Scope.** The full description of the settlement: all sections and types of record. Town, suburb and the spatial meaning of a section are not merged by assumption. |
| F02 | **Units.** Entry, property yard, enumerated residential group, family and reconstructed household are different units; no correspondence is assumed. |
| F03 | **Value and status.** A number or text is separate from its status. Biographical characteristics have their own status fields. |
| F04 | **Keeping the source.** The supplied files and full machine text are kept. Working `*_raw` fields hold a readable extract or summary; no diplomatic transcription is assumed. Every substantive record has a direct or inherited path to the source. |
| F05 | **Versions of the source.** Different witnesses are kept apart and the choice of a reading is explained. *(In Poltava, superseded in part by A17: only the PDF edition is a witness.)* |
| F06 | **Time.** Date of record, date of addition, date of event and term of agreement differ. A refinement does not rewrite the original layer. Dates are ISO intervals in the calendar of the source. |
| F07 | **Identity.** `ACTORS` holds people, institutions and collectives; `PERSON_FACTS` only people in a layer. Candidate identities across entries are explicit; name coincidence does not merge automatically. |
| F08 | **Residents.** Owner, informant, relative, signer and a mentioned dead person are not residents automatically. Residence, presence and manner of mention are separate. Population is counted for a defined selection and time. |
| F09 | **Family.** Pair links are kept independently of groups. Parenthood and marriage are not inferred from age or order alone. Household indicators are built only for a defined co-residence group and time. |
| F10 | **Property and measures.** Object, characteristic and right are not merged. Components are not added to their complex. |
| F11 | **Economic statements.** Each has a type and parties. A group sum is not split into individual rates. Half-yearly and weekly pay are not replaced by yearly. |
| F12 | **Health and height.** The historical formula and individual manifestations are kept. No diagnosis or inability without direct evidence. A group formula is not silently extended. |
| F13 | **Social categories and translation.** Estate, rank, occupation, office, dependency, wealth and HISCLASS class are different. English equivalents and HISCO come with a glossary, context and mapping status. |
| F14 | **Space.** Source toponym, modern identification, coordinate and yard location are different levels. |
| F15 | **Coverage.** Completeness is judged against the information actually present, not the share of filled cells. |
| F16 | **Manual editing.** Excel is the main working file. IDs are stable. Substantive edits get an explanation, new evidence or a rule reference. |
| F17 | **Analytical extracts.** An extract is a reproducible output with a release date, population, time policy, unknown-value rules and filters. |
| F18 | **Evolution.** The core fixes units, keys, time model and relations. A new phenomenon is first tested against existing fields; a new field or table is added with a versioned dictionary change, without losing old values. |
