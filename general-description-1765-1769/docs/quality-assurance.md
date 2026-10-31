# Quality assurance

Four levels of control were used. None of them replaces the others: a structural pass says nothing about whether a reading is right, and a source audit says nothing about broken keys.

## 1. Structural checks (every block)

Run on the saved workbook, not on the working model:

1. **Sheets and headers** are unchanged: the same 27 sheets, the same tables and fields in the same order.
2. **Code lists are unchanged**, except additions sanctioned by the researcher (in Poltava, exactly one: `life_event` = `birth`).
3. **Primary keys** are present and unique in every table.
4. **Foreign keys** resolve, using the references declared in the field dictionary.
5. **Code values** belong to their code list. *(B39)*
6. **No Latin look-alike letters** inside Cyrillic names (editorial markers in square brackets excepted).
7. **No formula errors** after a full recalculation.
8. **Formula ranges reach beyond the last row** of the table they reference, and derived columns are filled for every new row. *(B34)*
9. **Earlier rows are unchanged**, except corrections that are logged in `CHANGE_NOTES` with their previous state.
10. **No entry is coded twice.**

Further data rules checked by script: date intervals with start not after end; age bounds consistent; unknown age with no invented number; person profiles only for people; one profile per person and layer; no duplicate memberships, parties, events or coverage records; a party refers to an actor or a group, not both, and belongs to the same entry; numeric amounts consistent with their status; no cycles among property complexes or composite economic statements; trace records refer to existing records, fields, evidence and rules; no text longer than an Excel cell (32,767 characters).

## 2. Plausibility checks (every block)

Flags for review, not automatic corrections:

- parent–child age gaps under 12 or over 55 years; impossible or missing ages with a positive status; sex inconsistent with role or kin term;
- entries with no persons; yards with listed people but nobody resident;
- duplicate persons or profiles; enumerated groups without a head;
- hiring with no worker; money with a number but no unit, or a positive status with no number;
- yearly wages in rubles far outside the distribution of the base;
- layer dates out of sequence along the route of the source.

## 3. Independent source audit

The decisive check is a comparison of the base with **expectations written directly from the source** by an auditor who did not code that material.

1. **Sample.** Draw a stratified sample of entries (by section and block) with a fixed random seed, so the sample can be reproduced.
2. **Expectations from the page.** For each entry the auditor reads the page image at 200 dpi and writes down, without looking at the base:
   - residents of the original layer with name, age, sex and role;
   - people only mentioned (owner, signer, seller, relative);
   - buildings and their counts;
   - hiring terms (who, how much, per what period);
   - health formulas;
   - who signed for whom.
3. **Automatic comparison.** A script matches expected residents to the base by normalised name (old letters folded) and age, and lists missing people, extra people and differences of age, sex, role and status.
4. **Classification.** Each finding is graded critical (a missing or extra person, wrong age, sex or role), medium or minor, and either corrected with a change note or explained.
5. **Interval, not a point.** Report error shares with a 95% Wilson confidence interval. Zero errors in a sample still gives an upper bound, not a proof.
6. **Clusters lead to full re-checks.** If errors cluster in one block, that block is re-checked entry by entry against the images, and the error types found are then searched for across the whole base (for example plural "they have children" formulas, signing formulas, mixed Latin and Cyrillic letters).
7. **Age sample.** Where two versions of the text disagree on ages, a stratified sample of the disagreements is checked against the image.

A comparison of two summaries produced from the same model is not an independent check.

## 4. Counts from the text

For each block, the coder records control figures taken directly from the text: the number of residents of the original layer, totals of property, sums and periods, the composition of groups, disputed formulas. After the block, the same figures are computed from the base. Differences are explained, not adjusted away (for example, a later re-check moved two people written in darker ink into an uncertain layer).

## 5. Checking the saved file

- Read the **exported** workbook again; compare all primary values with the verified model and all derived values with their expected results.
- Check formulas, absence of errors, tables, filters, input validation and the highlighting of duplicates.
- In a test copy, enter a wrong age in a new row and confirm that the control reacts; restore the original.
- Look at several changed ranges visually after export.
- Do not claim a check in desktop Excel or in R unless it was actually run there.

## 6. Releasing a new master

1. Build the fix in a scratch copy, never in the master.
2. Run the structural QA; a candidate that fails is never promoted. *(C4)*
3. Verify the SHA-256 of the current master on disk, so that a manual edit made in the meantime is not overwritten.
4. Back up the current master; write the new one; verify its SHA-256 on disk.
5. Keep one rolling backup.
6. Pack a control package (master, rules, ID counters, scripts, audit logs, code books, checksums of every file) to a temporary file, check its integrity, then replace the previous package.

Neither an exit code nor the existence of a file proves integrity. After a crash, read the actual file before doing anything else.

## 7. Release checks

The public release is checked separately: the data dictionary matches the tables and columns; every row of every table has a description; no source text, internal journal or working note is present; keys and foreign keys resolve with the settlement prefix.
