# Workflow

How the coding of Poltava was organised, and what we learned. The method does not depend on AI; a human team can follow the same cycle. But the Poltava base was coded by AI agents under the researcher's supervision, and several rules below exist because of that.

## 1. Roles

- **The researcher** is the authority on the source. They set the standard rules, decide every genuinely ambiguous reading or identification, and approve any change to the structure or the code lists.
- **The coordinator** (an AI agent) plans blocks, applies the rules, keeps the decision log and the master, runs QA and asks the researcher only what the rules cannot decide.
- **Coding agents** code one block each, in a scratch copy, following the manual and the log.
- **Auditor agents** check samples against the page images. An auditor never checks material it coded.

## 2. The block cycle

1. **Take stock.** Get the current master, record its version, size and SHA-256, and keep an untouched copy as the input of the block. If the researcher edited the workbook by hand, carry those edits into the working model before anything else.
2. **Define the block.** About 25 entries; fewer when the text is dense. Make sure the pages of the whole block are at hand.
3. **Code in a fixed order:** entries; layers and dates; evidence texts; participants and person profiles; groups, memberships, residence and kin links; property and quantities; economic statements and parties; health and events; places and terms; statuses of unknown values and coverage; trace, review and change notes.
4. **Record control figures** from the text (residents, property, sums, groups).
5. **QA** ([`quality-assurance.md`](quality-assurance.md)): structural checks, plausibility checks, comparison with the control figures.
6. **Questions.** Only real ambiguities go to the researcher, one at a time, with the passage and the options. The answer becomes a numbered decision in the log.
7. **Release** the new master by the protocol in QA section 6, and pack a control package.

## 3. The decision log

The log is the memory of the project. It is read first by anyone who joins.

- **A** — standard rules from the researcher.
- **B** — decisions made during coding: `B# | entries | date | decision | source basis | superseded: —`.
- **C** — technical rules that do not change the meaning of the source.

Old decisions are never deleted; a change is written as a new decision that names what it supersedes. When a rule changes, it is applied to the whole base, not just to the blocks that follow, and the corrections are logged.

## 4. Working with AI agents: what proved necessary

- **Write the rules down before scaling up.** A pilot of about 100 entries with the researcher confirming readings produced most of the standard rules. Only then were blocks handed to agents.
- **Agents decide by rule, not by asking.** The researcher's time goes to source questions without precedent. Routine technical steps are not put to the researcher.
- **Independent audits find what self-checks miss.** In Poltava, an audit by agents not involved in the coding found that one late block had systematically lost mother–child links and signing records. The whole block was re-checked against the images, and the error types were then searched for across the entire base.
- **Never trust a summary of your own work.** A comparison of two figures produced by the same model is not a check. Expected values must come from the page.
- **Never trust an exit code.** Large workbook exports were killed by memory limits after the file had been written, and once before final patches were applied. The saved file must be read back and compared every time.
- **Formula ranges silently stop.** Derived columns referenced fixed ranges and stopped working for new rows. QA now checks that every range reaches beyond the data.
- **Text caches lie at the edges.** A heading with no space after the number merged two entries in an early text cache. A missing key in a cache is not a missing entry.
- **Keep one master.** One file, one identity, one rolling backup, and a checksum check before every replacement so that the researcher's manual edits are never overwritten.
- **Reusable code carries old cases.** Helper functions accumulate conditions for specific entries. Read them before reuse; never run an old preparation script blindly.

## 5. Hand-over

At the end of a working stage, a control package is packed for the next coder:

- the master and its checksum;
- the decision log and the ID counters;
- the scripts (builder, QA, locators, text extraction, merging of participants, release);
- audit logs (samples, expectations, comparisons, findings);
- the code books in Ukrainian and English;
- checksums of every file and the exact next step.

A short hand-over note states what is finished, what is a candidate only, which questions are open, and where to start.
