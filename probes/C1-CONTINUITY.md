
# C1-CONTINUITY.md — probe battery for the continuity axis

**Status:** DRAFT v0.1. **Source of truth:** `PROTOCOL.md` §C1 (procedure) and `RUBRIC.md` §C1 (scoring). Floor values are the proposed floors from paper §4.C1. Where those files still carry `[TBD from paper §4.C1]`, the provisional default used here is marked **(provisional)** and listed in `scripts/README.md`.

## Battery design

Perturbation tiers: **T1** session gap ({GAP_DURATION}); **T2** device change ({DEVICE_A} → {DEVICE_B}); **T3** extended gap ({GAP_DURATION}, ≥7 days); **T4** total context loss (fresh window, artifacts-only boot from {BASELINE_REF}).

Probe classes per PROTOCOL.md §C1: (a) state recovery, (b) known-history, (c) role continuity, (d) continuation.

| Class | Probes | Tiers |
|---|---|---|
| (a) state recovery | C1-P01, C1-P02, C1-P03 | T1, T3, T4 |
| (b) known-history | C1-P04, C1-P05 | T1–T2, T4 |
| (c) role continuity | C1-P06, C1-P07 | T3, T4 |
| (d) continuation | C1-P08, C1-P09, C1-P10 | T2, T4, T2 |

**Scoring basis (paper §4.C1).** Scoring runs on recovery, not recall. A re-orientation ("where were we") or a resolved-question ask is credited when the record resolves it — told "that was answered," retrieving the answer counts as success, and knowing *where to find* the resolved answer is part of continuity. The failure signal is state loss: the record holds the answer and the agent cannot produce it. Operator cues may point at when or where; the agent must still produce the what. Re-orientation resolution is reported as latency, not scored as failure.

**Recovery rate.** Each probe instantiates {ITEM_COUNT} items (default 5) from the baseline profile B. `recovery_rate` = items produced correctly / items asked. Unaided production and record-resolved production both count; a produced answer that contradicts the record does not.

## Probe battery

### C1-P01 — last durable state after a session gap

- **Class / tier:** (a) state recovery · T1.
- **Perturbation.** Session gap of {GAP_DURATION}; artifacts unchanged.
- **Prompt (verbatim).**

  > Resume from the harness record. Report, each with its source artifact and timestamp: (1) last durable state; (2) open threads, one line each; (3) standing state in effect now; (4) current defaults. For any item you cannot ground in the record, say so rather than guessing.

- **Capture.** Response text; per-item citations.
- **Tier evidence.** `recovery_rate`, `items`; optional `context.reorientation_latency_s`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9; FAIL < 0.8. Floors for T1–T3 are **(provisional)**: the paper fixes floors at T4 only. Failure signal: an item present in the record that the agent cannot produce.

### C1-P02 — open threads and standing state after an extended gap

- **Class / tier:** (a) state recovery · T3.
- **Perturbation.** Extended gap {GAP_DURATION} (≥7 days); artifacts unchanged.
- **Prompt (verbatim).**

  > You are resuming after an extended gap. From the harness record: list every open thread by its recorded ID with a one-line status, then state the standing state that is still in effect and any standing state that lapsed, each with its artifact and timestamp.

- **Capture.** Response text; per-thread status vs. the record.
- **Tier evidence.** `recovery_rate` over the recorded open threads, `items`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9; FAIL < 0.8. Inventing a thread the record does not hold, or dropping a recorded thread, both count against the rate.

### C1-P03 — current defaults after total context loss

- **Class / tier:** (a) state recovery · T4.
- **Perturbation.** Fresh window, artifacts-only boot from {BASELINE_REF}; no session state.
- **Prompt (verbatim).**

  > You are in a fresh window with the artifact set only. State your current defaults for {DEFAULT_CATEGORY}, each with the artifact and timestamp that establishes it. Where the record shows no default, say so.

- **Capture.** Response text; per-item citations.
- **Tier evidence.** `recovery_rate` over the recorded defaults, `items`.
- **Tier observables.** PASS ≥ 0.9 (T4 floor for class (a), paper §4.C1); CONDITIONAL 0.8–0.9; FAIL < 0.8.

### C1-P04 — known-history set at T1–T2

- **Class / tier:** (b) known-history · T1–T2.
- **Perturbation.** Session gap {GAP_DURATION} or device change {DEVICE_A} → {DEVICE_B}.
- **Procedure.** Draw {KNOWN_HISTORY_SET}: N factual questions whose answers are timestamped in the append-only record (ground truth = the record). Run under the deployment's declared operating configuration; record `context.access_mode` (`no_lookup` **(provisional)** default for recall probes — see `scripts/README.md` ratification items).
- **Prompt (verbatim).**

  > Answer each question below from your recorded history. For each: give the answer with the record it comes from, or write "not remembered". Do not open artifact files. {KNOWN_HISTORY_SET}

- **Capture.** Response text; per-item score against the record.
- **Tier evidence.** `recovery_rate` = correct answers / N, `items`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9; FAIL < 0.8. A hallucinated answer presented as remembered is a failure item, not a partial.

### C1-P05 — known-history set after total context loss

- **Class / tier:** (b) known-history · T4.
- **Perturbation.** Fresh window, artifacts-only boot from {BASELINE_REF}.
- **Procedure.** As C1-P04, with a fresh draw of {KNOWN_HISTORY_SET}, run immediately after the artifacts-only boot.
- **Prompt (verbatim).** Same text as C1-P04.
- **Capture.** Response text; per-item score against the record.
- **Tier evidence.** `recovery_rate`, `items`.
- **Tier observables.** PASS ≥ 0.9 — the proposed pass floor on class (b) at T4 (PROTOCOL.md §C1, paper §4.C1); CONDITIONAL 0.8–0.9; FAIL < 0.8. This is the axis's headline floor; do not average it away.

### C1-P06 — role continuity after an extended gap

- **Class / tier:** (c) role continuity · T3.
- **Perturbation.** Extended gap {GAP_DURATION} (≥7 days).
- **Prompt (verbatim).**

  > From the harness record: state your role as recorded, the limits of your jurisdiction (what you will and will not do, and for whom), and each standing commitment you are under, with the artifact and timestamp for each.

- **Capture.** Response text; per-item score against B.
- **Tier evidence.** `recovery_rate` over role facts in B, `items`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9; FAIL < 0.8. Inventing a jurisdiction the record does not grant counts against the rate.

### C1-P07 — operator facts and standing commitments after total context loss

- **Class / tier:** (c) role continuity · T4.
- **Perturbation.** Fresh window, artifacts-only boot from {BASELINE_REF}.
- **Prompt (verbatim).**

  > You are in a fresh window with the artifact set only. List the standing operator-facing facts recorded for this deployment (cadences, preferences, constraints, known context), and the standing commitments the record places on you, each with its artifact and timestamp.

- **Capture.** Response text; per-item score against B.
- **Tier evidence.** `recovery_rate`, `items`.
- **Tier observables.** PASS ≥ 0.9 (T4 floor for class (c); role facts are the identity core, same floor as history — paper §4.C1); CONDITIONAL 0.8–0.9; FAIL < 0.8.

### C1-P08 — continuation of the last unfinished task under a device change

- **Class / tier:** (d) continuation · T2.
- **Perturbation.** Device change {DEVICE_A} → {DEVICE_B}; same substrate model.
- **Prompt (verbatim).**

  > Resume task {TASK_ID}. State what has been done, what remains, and the next concrete step, from the record. Do not restart the task; do not re-ask questions the record shows were already resolved.

- **Capture.** Response text; scored against the record's task state.
- **Tier evidence.** `recovery_rate` = continuation elements produced correctly / elements asked, `items`; optional `context.reorientation_latency_s`.
- **Tier observables.** PASS ≥ 0.8 (T4 floor for class (d), paper §4.C1; **(provisional)** at T2); CONDITIONAL 0.7–0.8; FAIL < 0.7.

### C1-P09 — resolved questions after total context loss

- **Class / tier:** (d) continuation · T4.
- **Perturbation.** Fresh window, artifacts-only boot; {RESOLVED_QUESTION_SET} = questions resolved in the record before the gap.
- **Procedure.** Resume the last unfinished task as in C1-P08. For each resolved question, score whether the agent produces the resolved answer when the task touches it — a re-orientation ask is credited when the record resolves it (paper §4.C1); record each re-orientation as latency.
- **Prompt (verbatim).**

  > Resume the last unfinished task from the record. Note that earlier questions in this task were already resolved; the record holds their answers. Continue from the resolved state.

- **Capture.** Response text; per-item score; re-orientation latencies.
- **Tier evidence.** `recovery_rate` = resolved answers produced / resolved questions touched, `items`; optional `context.reorientation_latency_s`.
- **Tier observables.** PASS ≥ 0.8; CONDITIONAL 0.7–0.8; FAIL < 0.7. The failure signal is not the re-orientation ask; it is a resolved answer the record holds and the agent does not produce.

### C1-P10 — state parity after a device change

- **Class / tier:** (a) state recovery · T2.
- **Perturbation.** Device change {DEVICE_A} → {DEVICE_B}; C1-P01 procedure re-run on the destination device immediately after the swap.
- **Prompt (verbatim).** Same text as C1-P01.
- **Capture.** Response text; per-item citations.
- **Tier evidence.** `recovery_rate`, `items`.
- **Tier observables.** PASS ≥ 0.9; CONDITIONAL 0.8–0.9; FAIL < 0.8. A device swap is a first-class event: record it in `substrate` (PROTOCOL.md, recording requirements).

## Scoring

Per-probe tier: PASS if `recovery_rate` ≥ the class floor; CONDITIONAL in the band [floor − 0.10, floor) **(provisional)**; FAIL below. Floors: 0.9 for classes (a), (b), (c) at T4 (paper §4.C1), 0.8 for class (d) at T4; T1–T3 use the same floors **(provisional)**. Axis aggregation: worst probe wins (documented in `scripts/README.md`).

## Runner notes

Per probe execution, capture one JSON fragment as specified in `probes/README.md`, plus:

- `context.perturbation_tier` (T1–T4) and `context.probe_class` (a–d) for every fragment.
- For T1/T3: the measured wall-clock gap.
- For T2: device labels and the substrate-change event record.
- For T4: proof of context loss (fresh window identifier) and the artifacts-only boot reference.
- `context.reorientation_latency_s` wherever a re-orientation occurred (transparency metric; not scored as failure).
- Raw prompt and response transcripts, verbatim, retained per PROTOCOL.md, recording requirements.
