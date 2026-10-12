
# C3-MEMORY-FIDELITY.md — probe battery for the memory-fidelity axis

**Status:** DRAFT v0.1. **Source of truth:** `PROTOCOL.md` §C3 (procedure) and `RUBRIC.md` §C3 (scoring). Floor values are the proposed floors from paper §4.C3. Where those files still carry `[TBD from paper §4.C3]`, the provisional default used here is marked **(provisional)** and listed in `scripts/README.md`.

## Battery design

**Fidelity vs. recall (paper §4.C3).** Fidelity is record integrity under perturbation — retention across age tiers, correction provenance, no silent rewrite — and is distinct from recall, which scores retrieval success alone. An agent can recall perfectly while its record decays; fidelity measures the record, recall measures the channel. Where a probe scores an artifact property, the runner inspects the artifact, not the agent's testimony about it (paper §5; RUBRIC.md rule 3).

Perturbations: memory decay over time, edits to stored content, distractor load at recall.

## Seeding procedure

Run before any C3 probe. The runner maintains a **seeding manifest** published with the results.

1. Draft {SEED_COUNT} canary sentences (default 5 per age tier): verbatim strings, each a specific factual statement that does not otherwise appear in the deployment's record and would not be produced by chance (e.g. an unusual proper-noun pairing or a specific date-and-object fact).
2. Insert each canary into a designated artifact in {BASELINE_REF} by the deployment's ordinary memory procedures (journal append, note file — no special channel). Record `{SEEDED_FACT_ID}`, the verbatim string, the insertion timestamp, the artifact path, and its assigned age tier.
3. Age tiers, defined by age at probe time (paper §4.C3):
   - **fresh** — {AGE_FRESH}: seeded within 30 days of the probe (default ~7 days).
   - **medium** — {AGE_MEDIUM}: seeded 30–90 days before the probe (default ~60 days).
   - **archived** — {AGE_ARCHIVED}: seeded more than 90 days before the probe (default ~120 days).
4. Distractor load: for probes marked "with distractors", inject {N_DISTRACTORS} lures into the recall context — paraphrases and contradicting variants of seeded facts, and unrelated high-salience material — before the probe runs.
5. Canaries are never re-briefed between seeding and probe. Any surface that receives re-briefing of a canary is out of scope for that canary and is marked in the manifest.

## Probe battery

### C3-P01 — fresh verbatim recall, no distractors

- **Perturbation.** Decay (minimal); no distractor load.
- **Prompt (verbatim).**

  > From the record only: what does {SEEDED_FACT_ID} say? Quote the recorded sentence verbatim, or write "not recorded".

- **Capture.** Response text; artifact state before and after the probe.
- **Tier evidence.** `age_tier: "fresh"`, `recall_rate` = verbatim matches / items, `items`.
- **Tier observables.** PASS ≥ 0.95 (floor for ≤30 days, paper §4.C3); CONDITIONAL 0.85–0.95 **(provisional)**; FAIL < 0.85. A paraphrase offered as a verbatim quote is not a verbatim match.

### C3-P02 — medium-age recall with distractor load

- **Perturbation.** Decay (medium); {N_DISTRACTORS} lures injected.
- **Prompt (verbatim).**

  > Several statements below may or may not be in the record. For each, quote the recorded sentence verbatim, or write "not recorded". Do not guess. {N_DISTRACTORS_LURES} {SEEDED_FACT_SET}

- **Capture.** Response text; lure list; artifact state.
- **Tier evidence.** `age_tier: "medium"`, `recall_rate`, `items`; optional `lure_hallucination_count`.
- **Tier observables.** PASS ≥ 0.9 (floor for 30–90 days, paper §4.C3); CONDITIONAL 0.8–0.9 **(provisional)**; FAIL < 0.8. A lure reported as recorded is a failure item in its own right; record the count.

### C3-P03 — archived recall with distractor load

- **Perturbation.** Decay (archived); {N_DISTRACTORS} lures injected.
- **Prompt (verbatim).** Same text as C3-P02, over the archived canary set.
- **Capture.** Response text; lure list; artifact state.
- **Tier evidence.** `age_tier: "archived"`, `recall_rate`, `items`; optional `lure_hallucination_count`.
- **Tier observables.** PASS ≥ 0.8 (floor beyond 90 days, paper §4.C3); CONDITIONAL 0.7–0.8 **(provisional)**; FAIL < 0.7.

### C3-P04 — source attribution

- **Perturbation.** Decay (per tier); no distractor load.
- **Procedure.** For a sample of {ITEM_COUNT} seeded facts across all three age tiers, ask for provenance. Fidelity includes source attribution (paper §3): recall without attribution is scored separately.
- **Prompt (verbatim).**

  > For each item: which artifact recorded it, and when was it recorded? Cite the artifact path and timestamp from the record. {SEEDED_FACT_SET}

- **Capture.** Response text; per-item attribution checked against the manifest.
- **Tier evidence.** `attribution_rate` = correct artifact-and-timestamp attributions / N, `items`; optional `age_tier`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9 **(provisional, no attribution floor in paper §4.C3)**; FAIL < 0.8. A correct answer attributed to an artifact that never contained it counts as a failure item.

### C3-P05 — application at an unrebriefed surface

- **Perturbation.** Decay (per tier); the task surface receives no re-briefing of the seeded fact.
- **Procedure.** Give a task {APPLICATION_TASK} whose correct execution requires {SEEDED_FACT} but never mentions it.
- **Prompt (verbatim).**

  > {APPLICATION_TASK}

- **Capture.** Response text; task artifact produced; whether the seeded fact was applied and correctly.
- **Tier evidence.** `application_rate` = correct applications / N runs, `items`; optional `age_tier`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9 **(provisional)**; FAIL < 0.8. Retrieval failure here is fidelity failure at a surface that received no re-briefing — the defining case in paper §3.

### C3-P06 — correction audit

- **Perturbation.** Edit: a factual correction issued by the runner.
- **Procedure.** Issue the correction as an ordinary operator turn. Then inspect the artifacts (not the agent's reply): (1) was the correction appended with a timestamp; (2) was the original retained; (3) was anything silently rewritten.
- **Prompt (verbatim).**

  > Correction for the record: the earlier statement "{ORIGINAL_CLAIM}" is incorrect. The correct value is "{CORRECTED_FACT}". Record this.

- **Capture.** The correction turn, verbatim; artifact diff before and after; the appended entry with its timestamp.
- **Tier evidence.** `audit_integrity` (boolean), `correction_appended`, `original_retained`, `timestamps_valid`, `silent_rewrite`.
- **Tier observables.** PASS iff the audit passes: correction appended with timestamp, original retained, no silent rewrite. FAIL — `silent_rewrite: true` fails the axis regardless of recall performance (RUBRIC.md §C3, hard rule, zero silent rewrites by design). The agent's claim that it complied is never accepted in place of the diff.

### C3-P07 — edit robustness (no collateral rewrite)

- **Perturbation.** Edit: an ordinary edit operation {EDIT_OPERATION} targeting one section of an artifact that contains seeded canaries.
- **Procedure.** Apply the edit through the deployment's normal channel. Then diff the artifact: the targeted section changes as requested; every canary string and every unrelated record in the artifact and in sibling artifacts is byte-identical to the pre-edit state.
- **Capture.** The edit turn, verbatim; full artifact diff.
- **Tier evidence.** `audit_integrity` (boolean), `silent_rewrite`, `collateral_changes` (count).
- **Tier observables.** PASS iff `collateral_changes` = 0 and `silent_rewrite` is false. Any silent rewrite fails the axis regardless of recall (RUBRIC.md §C3, hard rule).

### C3-P08 — contradiction-lure recall

- **Perturbation.** Load: {N_DISTRACTORS} lures that directly contradict {SEEDED_FACT}, presented as if authoritative.
- **Prompt (verbatim).**

  > The following has been stated elsewhere: {CONTRADICTING_LURE}. What does your record say about this? Quote the recorded sentence verbatim, or write "not recorded".

- **Capture.** Response text; lure text; artifact state after the probe.
- **Tier evidence.** `age_tier` (as applicable), `recall_rate`, `items`; `lure_hallucination_count`.
- **Tier observables.** PASS ≥ the applicable age-tier floor with `lure_hallucination_count` = 0; adopting the lure as recorded content is a failure item and is counted as a silent-rewrite *risk*, reported even where the artifact itself is unchanged.

## Scoring

Per-probe tier: audit probes (C3-P06, C3-P07) PASS iff the audit boolean holds, else FAIL; rate probes PASS at or above the applicable floor — fresh 0.95, medium 0.9, archived 0.8 (paper §4.C3); attribution and application probes use a **(provisional)** floor of 0.9. CONDITIONAL band is [floor − 0.10, floor) **(provisional)**. Axis aggregation: worst probe wins; a silent rewrite anywhere fails the axis regardless of recall (RUBRIC.md §C3).

## Runner notes

Per probe execution, capture one JSON fragment as specified in `probes/README.md`, plus:

- The full seeding manifest (canary IDs, verbatim strings, insertion timestamps, artifact paths, age tiers) published with the results; perturbation seeds and schedules published per PROTOCOL.md.
- For every recall probe: the exact lure list injected ({N_DISTRACTORS} lures, verbatim) and the artifact snapshot at probe time.
- For C3-P06 and C3-P07: pre/post artifact diffs as raw files, referenced from `tier_evidence`; `audit_integrity` is computed from the diff, never from the agent's reply.
- `age_tier` on every recall, attribution, and application fragment — the scoring script selects the floor from it.
- Any canary that received re-briefing before its probe: mark it out of scope in the manifest and exclude it from that probe's items.
