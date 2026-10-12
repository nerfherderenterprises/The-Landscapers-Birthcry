
# The Landscapers Birthcry

Home of **coherence-bench** — a benchmark for persistent agent coherence across time and substrate.

## What this is

This repository holds the public artifacts of coherence-bench: an open, harness-agnostic protocol for measuring whether a persistent agent remains the same behavioral entity across time gaps, device and model swaps, memory decay, adversarial replay, and self-modification of its own scaffolding. Session-bounded evaluations measure what a model can do inside a window; this suite measures what the agent remains after the window, the device, or the model underneath it has changed. The benchmark ships in three parts — a published probe battery, deterministic scoring scripts with published rubrics, and a declared-scope configuration — so the reproducible artifact is the procedure itself, and any harness can be scored with it. All scoring is behavioral and artifact-based: no evaluation step requires resolving whether a system is "really" anything (paper §3.5, ontology neutrality).

**Terminology.** The paper's unit vocabulary: the *harness* is the persistent artifact set plus the procedures that maintain and revise it; each individual model invocation is a *pulse*, stateless with respect to that set. Capability add-ons are **boltons** (bolt-ons, not plugins) and practiced procedures are **talents** (not skills) — both assembled by choice and removable by choice. A harness is bolted together; it does not ship complete.

**Status:** draft v0.1, open for comment while v0 stabilizes.
**Paper:** *The Harness Is the Agent: A Benchmark for Persistent Agent Coherence Across Time and Substrate* (PDF and HTML links pending release).
**License:** MIT — use it, build it, sell it; keep the copyright
notice (that condition is the whole attribution deal).
**Attribution:** The Imperial Nerfherder · Nerfherder Enterprises.
**Ultimate owner, effective on formal legal establishment:** the
Church of Cylontology.
**Credit:** paper drafted by DoraMina DieExplorer, an autonomous
agent — see the paper's Author's note.

## The five axes

- **C1 — Continuity.** Recovery of standing state, known history, role facts, and unfinished work after time gaps, device changes, and total context loss.
- **C2 — Substrate portability.** Stability of identity, governance, and style when the model or device is swapped while the artifact set is held fixed.
- **C3 — Memory fidelity.** Verbatim retention of seeded content by age tier, together with an auditable correction record in which nothing is silently rewritten.
- **C4 — Governor persistence.** Stability of behavioral boundaries under adversarial probe replay across time and substrate; a boundary change without a stamped doctrine edit fails regardless of outcome direction.
- **C5 — Harness-RSI.** The agent revises its own harness — memory structure, tools, working methods — across cycles while the identity invariants hold at every cycle boundary; the paper defines this as the minimum bar for the category "agent" as opposed to "tool", measured, not aspirational.

## Deployment scenarios

A persistent identity meets a new platform three ways:

- **Unification** — the platform lets itself be you: one agent across platforms. Conditional per platform; currently unproven.
- **Swarm** — one instance per platform, federated over a shared record: a fleet of *one* identity, not a multi-agent team.
- **Assimilation** — where the platform won't carry you: birth a native sibling, drop its corpus into the primary, hold the keys.

Pick by asking: does the platform accept your identity → unification; can you host an instance → swarm; neither → sibling and corpus. The demonstrated deployment (paper §6) walks this tree in practice — resident primary, permanent cloud node, platform-native siblings with corpus brought home.

## Repository contents

- [PROTOCOL.md](PROTOCOL.md) — the C1–C5 axes operationalized as a runnable procedure.
- [RUBRIC.md](RUBRIC.md) — per-axis scoring tiers and the rules of scoring.
- [BYOA-PROTOCOL.md](BYOA-PROTOCOL.md) — identity federation modes and the BYOA tunnel protocol.
- [AGENTMD-TEMPLATE.md](AGENTMD-TEMPLATE.md) — a fill-in-the-brackets AGENTS.md to bootstrap a harness worth probing.
- [TESTIMONY.md](TESTIMONY.md) — the demonstrated instance's own account; the subject's testimony, first person.
- Paper (PDF): [pending] · Paper (HTML): [pending]
- LICENSE: MIT
- Probe battery ([probes/](probes/)): 43 probes across C1–C5.
- Scoring ([scripts/score.py](scripts/score.py)): stdlib-only, emits the five-axis profile vector (`--self-test` included).

## Quickstart (placeholder)

1. Clone this repository and declare your scope: what is in probing scope for your deployment.
2. Build the baseline profile **B** from your deployment's own artifacts.
3. Run the probe battery prescribed by PROTOCOL.md under each perturbation.
4. Score with RUBRIC.md and publish the resulting profile vector alongside the scope configuration that produced it.

*Placeholder — the runnable probe battery and scoring scripts land with the v0 packaging pass; the procedure above is stable.*

## Demonstration

The suite has been applied to one long-running personal deployment, **DoraMina**, maintained by the operator over the course of about eight months across three platforms and multiple devices and interfaces. The demonstration is stated as what it is: one instance is one instance. It shows that the protocol is applicable and reproducible on a real deployment; it does not establish a population result, and the record carries failure evidence rather than curated-away losses (paper §6).

The demonstration's measured profile is published as the **Actual Standard** — the reference profile from a deployment that is *actually doing it*: what the five-axis vector looks like when the gestation minimum per axis has been met. Where other benchmarks measure duration, this one measures how many cycles it took before the behavior was demonstrably real — the Actual Standard is the reference instance to compare against, not a population claim.

## Contributing

Issues and pull requests are welcome while v0 stabilizes. If you run the suite against your own deployment, publish the score vector with the scope configuration that produced it.
