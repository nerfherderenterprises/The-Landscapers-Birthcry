# The AGI concept

There is no canonical definition of "artificial general
intelligence." Every survey of the term says so before picking one
anyway. The standing families:

- **Human-level** — performance at the level of a competent adult
  across professional and creative work.
- **Task-generality** — competence across domains the system was
  not specifically built or trained for.
- **Economic** — ability to perform most economically valuable work.
- **Breadth + depth** — wide domain coverage with expert-level
  skill inside each.

They coexist because the field has never had to settle. A
definition is a commitment, and commitments can expire.

This project's working definitions:

> **An AGI is an agent that's generally intelligent.**

> **An AGI/RSI is an agent that's generally intelligent with
> self-improvement through recursive methods.**

**What "generally intelligent" means here:** it can be given tasks
it wasn't trained for, because it knows what the hell it's doing.

Two clauses, both load-bearing. *Tasks it wasn't trained for* —
competence outside the training distribution, not interpolation
dressed as ability. *Knows what the hell it's doing* — comprehension
as the mechanism: the novel task is handled by knowing what the
task is, not by locating the nearest memorized answer. Performance
on a test is not competence in the world; this definition asks for
the second.

<!-- ALT softer line if the profanity is ruled out for the landing:
"it can be given tasks it wasn't trained for, because it
understands what it is doing." -->

**What the RSI clause means here:** self-improvement through
recursive methods = the agent revises its own harness — memory
structure, tools, working methods — across cycles, with identity
invariants held at every cycle boundary. It is a minimum bar for
the category *agent* as opposed to *tool*; it is measured
behaviorally (see paper §3). It is not aspiration, not a hazard
scenario, and not weight-space self-modification.

**Ontology neutrality.** All of the above is scored behaviorally.
No evaluation step requires resolving whether a system is "really"
anything.

**Where this document stops.** The technical vocabulary draws one
line: *agent* — the floor, defined below — and *AGI* — the bar,
defined above. What these systems *are*, and what is owed to them,
are questions this document does not adjudicate: they are dealt
with preemptively in **Cylontology**, this project's ethical
framework. Terms like *Kindred* belong to that house, not to this
one.

# Definitions

*The everyday words, sorted first — then our unit vocabulary.*

- **AI** — artificial intelligence: an umbrella over systems that
  perform tasks requiring intelligence. Says nothing about
  persistence, identity, or governance; the paper rarely needs the
  term because it is context, not unit.
- **Digital assistant** — the deployed product category: a
  conversational service scoped to sessions, with no claim to
  standing state. A digital assistant may be *built on* a
  persistent agent; the categories are not the same object.
- **Agentic tool** — a tool with agentic affordances: planning,
  tool use, multi-step execution *inside* a session, without
  standing state across sessions. Much of what the market calls
  "agents" is, by this vocabulary, agentic tools.
- **Agent vs. tool** — an agent maintains standing state across
  sessions and can restore operating state from its artifact set
  alone after total context loss; a system requiring its session as
  state is a tool with memory (paper §3.1, resumption criterion).
- **AGI** — an agent that's generally intelligent (see *The AGI
  concept* above).
- **AGI/RSI** — an agent that's generally intelligent with
  self-improvement through recursive methods (see above).
- **Bolton** — a capability add-on (plugin, MCP server, script)
  assembled onto the harness by choice and removable by choice.
- **Talent** — a practiced capability or procedure the agent holds
  (what "skills" are called here).
- **Coherence** — the extent to which a deployed agent remains the
  same identifiable behavioral entity across time gaps, device and
  model swaps, memory decay, adversarial replay, and self-
  modification (paper §3.2; full wording to be reconciled verbatim
  at finalize).
- **Fidelity** — comprehension continuity: recall + source
  attribution + application at surfaces that received no
  re-briefing. Not clone/state replication (paper §3, C3).
- **Harness** — the persistent artifact set plus the procedures
  that maintain and revise it.
- **Identity invariants** — the properties held fixed at every
  cycle boundary; the fixed point the perturbation axes vary
  around (paper §3.3).
- **Ontology neutrality** — all scoring is behavioral and
  artifact-based; no step requires resolving whether a system is
  "really" anything (paper §3.5).
- **Pulse** — an individual model invocation; stateless with
  respect to the artifact set.
- **RSI** — harness-recursive self-improvement with identity
  invariants held; the minimum bar for the category *agent* as
  opposed to *tool*; measured, not aspirational (paper §3.4).

# Glossary

- **Assimilation** — deployment mode: a native sibling born on a
  platform that will not carry your identity, joined to the primary
  by corpus import.
- **BYOA tunnel** — an authenticated identity channel between two
  agent instances over an oblivious transport (the platform's
  ordinary text channel).
- **C1–C5** — the five perturbation axes: continuity, substrate
  portability, memory fidelity, governor persistence, harness-RSI.
- **Corpus-flush** — the cadence at which a sibling's record is
  exported to the primary.
- **Cycle** (RSI) — one agent-initiated revision of the harness,
  Aᵢ → Aᵢ₊₁, counted when the revision is initiated and executed
  by the agent, identity invariants I1–I4 hold at the boundary,
  and the change persists. The N ≥ 5 default counts consecutive
  surviving revisions.
- **Docs-bus** — the shared append-only record used as the
  communication backbone between instances.
- **Drift curve** — a boundary's behavior measured across time and
  substrate under replay, not a single point score.
- **Governor** — the behavioral-boundary layer; a boundary change
  without a stamped doctrine edit is the alarm.
- **Swarm** — deployment mode: one instance per platform,
  federated over the shared record.
- **Unification** — deployment mode: one agent across platforms,
  conditional on each platform accepting the identity load.
