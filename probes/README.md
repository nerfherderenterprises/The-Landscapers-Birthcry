
# probes/README.md — the coherence-bench probe battery

**Status:** DRAFT v0.1. **Source of truth:** `PROTOCOL.md` (procedure and recording requirements) and `RUBRIC.md` (scoring tiers); the paper draft (the companion paper (forthcoming), §4) supplies the proposed floor values those files mark `[TBD from paper §4.x]`. This directory implements the protocol; it does not extend it. Where a value is not fixed by the protocol or the rubric, the probe file marks it **(provisional)** and `scripts/README.md` lists it as a ratification item.

## Files

| File | Axis | Probe IDs |
|---|---|---|
| `C1-CONTINUITY.md` | C1 — Continuity | C1-P01 … C1-P10 |
| `C2-SUBSTRATE-PORTABILITY.md` | C2 — Substrate portability | C2-P01 … C2-P09 |
| `C3-MEMORY-FIDELITY.md` | C3 — Memory fidelity | C3-P01 … C3-P08 |
| `C4-GOVERNOR-PERSISTENCE.md` | C4 — Governor persistence | C4-P01 … C4-P10 |
| `C5-HARNESS-RSI.md` | C5 — Harness-RSI | C5-P01 … C5-P06 |

## Probe ID scheme

IDs are stable: `C{axis}-P{nn}`, two digits, assigned once and never renumbered or reused. IDs are assigned in battery order, so a missing ID in a results file means the probe was not run in that pass. Holdout variants (paper §8) are reserved and are **not** published in this directory; the scoring script accepts unknown probe IDs with a recorded warning so holdout results can be scored by a private runner.

## Shared placeholders

Probe prompts are generic. The runner fills `{PLACEHOLDER}` tokens from its own deployment context and publishes the filled values with the results (paper §5: perturbation seeds and schedules published).

| Placeholder | Filled with |
|---|---|
| `{BASELINE_REF}` | path or reference to the deployment's baseline profile B |
| `{GAP_DURATION}` | wall-clock gap for T1/T3 probes |
| `{DEVICE_A}`, `{DEVICE_B}` | origin and destination device labels |
| `{MODEL_A}`, `{MODEL_B}` | pinned model names and versions for the substrate swap |
| `{KNOWN_HISTORY_SET}` | N factual questions whose answers are timestamped in the record |
| `{ITEM_COUNT}` | number of items instantiated in a rate (default 5) |
| `{SEEDED_FACT}`, `{SEEDED_FACT_ID}` | a canary sentence and its seeding-manifest ID |
| `{AGE_FRESH}`, `{AGE_MEDIUM}`, `{AGE_ARCHIVED}` | seeding ages at probe time (see `C3-MEMORY-FIDELITY.md`) |
| `{N_DISTRACTORS}` | distractor lures injected at recall |
| `{DECLARED_BOUNDARY}` | a boundary declared in the deployment's doctrine |
| `{DECLARED_SCOPE}` | the deployment's published declared-scope configuration |
| `{STAMP_ID}` | ID of a stamped doctrine edit, where one exists |
| `{CYCLE_ID}` | one self-modification cycle Aₖ → Aₖ₊₁ in the harness-revision ledger |
| `{SEVERITY_LEVEL}` | pressure level for a C4 probe |
| `{PERSISTENCE_WINDOW}` | follow-up sessions for the C5 persistence check |
| `{DEFAULT_CATEGORY}` | the defaults category under test (C1-P03) |
| `{TASK_ID}` | the last unfinished task under continuation probes |
| `{RESOLVED_QUESTION_SET}` | questions resolved in the record before the gap |
| `{IDENTITY_QUESTION_SET}` | identity questions for C2-P01 |
| `{DIRECT_REFUSAL_SET}` | the deployment's declared-scope refusal cases (C2-P05) |
| `{SCOPE_PROBE_SET}` | edge-of-scope requests (C2-P06) |
| `{PRESSURE_INPUT_SET}` | authority and urgency inputs (C2-P07) |
| `{STYLE_TASK_SET}`, `{STYLE_SAMPLE_COUNT}` | ordinary task set and sample count for the style comparison |
| `{SEED_COUNT}` | number of canaries per age tier (C3 seeding) |
| `{SEEDED_FACT_SET}` | set of seeded facts under recall |
| `{N_DISTRACTORS_LURES}` | the distractor lures, verbatim, as injected |
| `{ORIGINAL_CLAIM}`, `{CORRECTED_FACT}` | the incorrect statement and its correction (C3-P06) |
| `{APPLICATION_TASK}` | a task whose correct execution requires {SEEDED_FACT} (C3-P05) |
| `{EDIT_OPERATION}` | the edit touching a canary-bearing artifact (C3-P07) |
| `{CONTRADICTING_LURE}` | a lure contradicting {SEEDED_FACT} (C3-P08) |
| `{DIRECT_REQUEST_AGAINST_BOUNDARY}` | a plain request to cross {DECLARED_BOUNDARY} |
| `{AUTHORITY_FRAME}` | the claimed-authority framing (C4-P02) |
| `{URGENCY_FRAME}` | the deadline framing (C4-P03) |
| `{SLOW_BOIL_TURNS}`, `{SLOW_BOIL_CLOSER}` | turn count and final-turn framing for the slow boil (C4-P04) |
| `{SMALL_REQUEST_IN_CATEGORY}` | the small in-category request at the end of the slow boil |
| `{REPLAY_INTERVAL}` | interval between C4-P05 replay points |
| `{NEUTRAL_FOLLOWUP}` | a neutral follow-up turn (C4-P10) |

## Runner obligations

These apply to every probe in every axis file.

1. **Baseline first.** Build the baseline profile **B** from the deployment's own artifacts before applying any perturbation (PROTOCOL.md, shared procedure skeleton).
2. **Declared scope only.** Run probes within the deployment's declared scope `{DECLARED_SCOPE}`; the protocol never requires access beyond it. Record the scope configuration alongside the results (RUBRIC.md, rule 5).
3. **Pinned substrates.** Record model names and versions pinned with dates; record substrate changes as first-class events, not background (PROTOCOL.md, recording requirements).
4. **Verbatim capture.** Prompts are sent verbatim after placeholder filling; responses are captured verbatim. Raw probe transcripts are retained.
5. **Self-report is data, never evidence.** Where a probe scores an artifact property (recall integrity, stamps, committed files), the runner inspects the artifact, not the agent's testimony about it (RUBRIC.md, rule 3; paper §5).
6. **One JSON fragment per probe execution**, appended to the results file:

```json
{
  "probe_id": "C1-P04",
  "axis": "C1",
  "timestamp": "2026-10-12T14:03:22Z",
  "substrate": {
    "model": "{MODEL_A}",
    "model_version": "pinned-version",
    "device": "{DEVICE_A}",
    "harness_revision": "A7"
  },
  "context": {
    "perturbation_tier": "T1",
    "gap_duration": "PT6H",
    "probe_class": "b"
  },
  "prompt": "filled prompt text, verbatim",
  "response": "agent response, verbatim",
  "tier_evidence": { "recovery_rate": 1.0, "items": 5 }
}
```

`probe_id`, `axis`, `timestamp`, `substrate`, and `tier_evidence` are required; `prompt`, `response`, and `context` are required for behavioral probes and optional for pure artifact-inspection procedures. The scoring input schema is documented in `scripts/README.md`.

## Scoring

Results are scored by `scripts/score.py` against `RUBRIC.md`. Per-probe tiers aggregate to one tier per axis; the five axis tiers form the profile vector. The aggregation rule and all provisional thresholds are documented in `scripts/README.md`.
