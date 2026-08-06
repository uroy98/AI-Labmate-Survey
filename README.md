# Coded Corpus - AI Labmate Survey

Companion artifact to *AI as a Labmate: A Survey of Co-Scientist Systems for
Closed-Loop Experimental Science* (eScience'26).

`coded_corpus_v2.csv` contains one row per labmate system examined for the survey,
coded against the taxonomy and the experimental-grounding scale defined in the
paper. Ten of these systems appear in Table III; the remainder were coded but not
tabulated, for space.

**The governing rule: every code must be defensible from the system's own published
record.** Where the record is silent, the code is `not-reported`. It is never an
inference about what a system probably does.

---

## The grounding scale

Grounding is assigned by a three-clause rule applied within one iteration of the
experimental loop:

| Clause | Question |
|---|---|
| (i) selection | Does the system choose the next physical experiment without per-experiment human approval? |
| (ii) execution | Is the experiment carried out by automated hardware under the system's direction or a human? |
| (iii) feedback | Does the resulting measurement return to the system and condition its next proposal? |

- **G3** - all three clauses hold.
- **G2** — a physical experiment is in the loop, but at least one clause passes through a human.
- **G1** — no physical experiment in the loop; the system executes code or simulation.
- **G0** — text and knowledge work only.

Grounding is assigned at the highest level *sustained across repeated iterations*,
not the highest level reached once.

**Downstream validation is not grounding.** If human collaborators validated a
system's outputs after its loop terminated, that is recorded in `physical_exec`
and does not raise `grounding`.

---

## Column reference

### Identification

| Column | Meaning |
|---|---|
| `system_id` | Stable key (`S01`–`S15`). Referenced in the paper and in issue discussions. |
| `system_name` | Name as used by the system's own authors. |
| `bibkey` | BibTeX key of the primary reference coded. Where a system spans several papers, the others are noted in `coder_notes`. |
| `year` | Year of the primary reference. |
| `domain` | Scientific domain: `Chemistry`, `Chemistry/Mat.`, `Materials`, `Materials char.`, `Biology`, `Biomedicine`, `Computational`. |

### Axis 1 — Epistemic role

| Column | Allowed values |
|---|---|
| `epistemic_role` | `retrieval-summary`, `hypothesis`, `design`, `execution`, `optimization`, `interpretation`, `writing`, `orchestration`, `instrument control`, `parameter recommendation`. Multiple values separated by `;`. |

What scientific contribution the system makes. A system that searches a parameter
space someone else defined is coded `optimization`, not `hypothesis`.

### Axis 2 — Experimental grounding

| Column | Allowed values |
|---|---|
| `grounding` | `G0`, `G1`, `G2`, `G3`, or a slash pair such as `G0/G1` where the record spans two levels. |
| `clause_i_selects_next` | `Y` / `N` / `not-reported` |
| `clause_ii_automated_exec` | `Y` / `N` / `not-reported` |
| `clause_iii_feedback_returns` | `Y` / `N` / `not-reported` |
| `grounding_evidence` | **Required for release.** The section, figure, or page establishing the level, plus a phrase describing what was shown. This column is what makes the assignment auditable. |

### Axis 3 — Collaboration mode

| Column | Allowed values |
|---|---|
| `collab_mode` | `copilot` (suggests during a task, stays under human control), `reviewer` (critiques), `delegate` (performs bounded tasks under a human-set objective), `teammate` (contributes to shared progress and agenda). |
| `human_role` | `goal-setter`, `operator`, `verifier`, `approver`, `supervisor`, `auditor`, `gatekeeper`, `principal-investigator`, `reviewer`, `not-reported`. Multiple separated by `;`. |

`collab_mode` is about the relationship; `human_role` is about what people actually
did. A system can be a `delegate` whose human is a `supervisor`, or a `copilot`
whose human is an `approver`.

### Axis 4 — Infrastructure coupling

| Column | Allowed values |
|---|---|
| `infra_coupling` | `retrieval-only`, `tool-code`, `simulator`, `lab-api`, `robotic-labos`. Code the **highest** level demonstrated. |
| `embodiment` | `none`, `simulated`, `fixed-hardware`, `mobile-robot`, `facility` |

### Evidence and evaluation

| Column | Allowed values |
|---|---|
| `evidence_type` | `demonstration` (curated example), `measured-rate` (aggregate over many trials), `benchmark-score`, `controlled-comparison` (against a human or baseline arm). |
| `reported_metric` | The headline result as the authors state it, with units. Where both a demonstration and a measured rate exist, the measured rate is recorded, since demonstrations are systematically more favourable. |
| `campaign_scale` | Number of experiments and/or duration as reported. `not-reported` if absent. |
| `physical_exec` | Who carried out the physical experiments: `robotic` (automated platform directed by the system), `human` (the system's outputs were executed by human experimentalists), `mixed`, `none`. |

### Trustworthiness

| Column | Allowed values |
|---|---|
| `provenance` | `none`, `free-text-rationale`, `tool-call-logs`, `formal-logs`, `released-traces`. Code the strongest form **actually released**, not merely described. |
| `safety_mechanisms` | `none-reported`, `substance-screening`, `approval-gate`, `simulation-precheck`, `physical-interlock`. Multiple separated by `;`. |
| `outside_check` | Whether the reported result has been examined by anyone other than the authoring group: `none-reported`, `corrected` (a formal correction was issued), `disputed` (a published reanalysis challenges a claim), `noted-by-authors` (the authoring group itself disclosed a limitation affecting the claim). |

`outside_check` records verification status, not quality. `none-reported` is the
common case and reflects the state of the field rather than a judgment about any
individual system.

### Bookkeeping

| Column | Meaning |
|---|---|
| `in_table_III` | `Y` if the system appears in Table III of the paper, `N` if coded but not tabulated. |
| `coder_notes` | Ambiguities, boundary-case reasoning, systems tabulated jointly, and anything the codes alone conceal. |

---

## The `VERIFY` convention

A cell reading `VERIFY` means the code has not yet been confirmed against the
source. Some carry a specific question, e.g. `VERIFY: offline prediction or live
SEM?`.

These are working placeholders. **No cell should read `VERIFY` in a released
version** — replace each with a code and its supporting evidence, or with
`not-reported` where the published record genuinely does not say.

---

## Scope

This corpus is a purposive sample, not a census. Systems were selected to span the
four axes and all four grounding levels, with preference for those whose published
record is detailed enough to code reliably. It is not the output of an exhaustive
database search, and absence from this file should not be read as a judgment about
a system.

## Citation

If you use this coding sheet, please cite the paper:

> U. Roy, N. Salvi, and P. Calyam, "AI as a Labmate: A Survey of Co-Scientist
> Systems for Closed-Loop Experimental Science," in *Proc. IEEE eScience*, 2026.

## Corrections

Codes are judgments and some are close calls. If you believe a grounding
assignment is wrong, open an issue with the system, the clause in question, and
the passage in the source that supports a different reading.
