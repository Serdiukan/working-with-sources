# Deriving residents for demographic analysis

Person profiles are observations (person × layer), not people. Demographic counts need a defined population and a defined time. The residents table is a derived table, rebuilt from the master by script with no manual edits. It has one row per resident of the **original record** of each entry.

## Included

- **Layer.** Only the original layer of each entry (`layer_kind` = `initial`).
- **Group.** Only people with a membership in an enumerated residential group of the same entry, in that original layer. The root group is the yard; nested groups are huts or families.
- **Appearance in the source.** `enumeration` = `listed`.
- **Residence.** `explicit_resident` or `contextual_resident`.
- **Participant type.** People only; institutions and named collectives are excluded.
- **Specific unnamed people** ("unnamed wife", "daughter, name not given") are included and flagged.
- **Temporarily absent residents** are included and flagged.

## Excluded

- **Later layers.** People who appear only in additions, marginal notes, corrections or uncertain layers.
- **People only mentioned:** non-resident owners, people who signed for others, mentioned relatives, sellers and previous owners.
- **Crossed-out entries.**
- **Unnamed groups of unknown size.** They are never turned into individuals; the entry is flagged so that an analysis can treat it as undercounted.

## Counting each person once

A person may belong to the root group, a hut group and a family at the same time. Each person is counted once per entry. The table gives the deepest enumerated group and the person's role both in the hut and in the yard.

## Flags for analysis

- **Owner known only from the heading.** The head of the root group is the owner named in the heading, residence is only contextual, and age and health are not recorded (typically "he has workers" with no listing of the owner). Drop these rows for a strict count.
- **Roles:** head, worker, worker's relative, apprentice, subject, lodger — taken from roles in any group of the entry.
- **Dependency** = `subject` follows the source formula. It does not imply legally proven serfdom.

## Checking the table

The counts are compared with the control figures recorded by hand during coding (see [`quality-assurance.md`](quality-assurance.md), section 4). Every difference is explained individually, for example a later re-check that moved a passage in darker ink out of the original layer, or a specific unnamed person added under a refined rule.
