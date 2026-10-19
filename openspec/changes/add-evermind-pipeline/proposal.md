# Proposal

> Derived from `proposal-v3.md` (v3.0) — Phase 1 Define and Plan, approved by the
> Capstone Committee on 19 October 2026. Group 1, Netherlands cohort.

## Why

A support knowledge base can only answer what has been documented. When it has no
answer, the customer is handed to a call-center agent who resolves the question by
hand — and the same question returns tomorrow. Customers wait, agents repeat work,
and the recurrence itself is never captured as a signal. The knowledge-base content
owner, the internal customer of this work, has no reliable view of what recurs and
decides from anecdote what to update.

QDM's 1-10-100 rule frames the cost: $1 to do the work right now, $10 to correct it
later, $100 to replace a customer lost to the error. Answering the same question by
hand spends effort without creating a lasting improvement. EverMind turns recurrence
into evidence an owner can act on — either a new FAQ, or a business improvement
opportunity that no FAQ would fix.

## What Changes

A greenfield demonstration of a business decision-support process, over mocked data,
with a human owner holding every decision.

- **Ingest and screen mocked questions.** JSON records standing in for questions that
  would otherwise reach a call-center agent are validated and screened before any
  agent processing. Question text is treated as untrusted data throughout.
- **Identify recurring needs.** Deterministic grouping of similar questions, with
  recurrence counted as a fact rather than estimated.
- **Check existing knowledge and cause.** Each need is compared against the mock FAQ
  catalogue and the approved reference corpus to decide whether it is already
  answered, answerable from approved evidence, out of scope, unsupported, or a
  business problem rather than a documentation gap.
- **Prepare reviewable recommendations.** Evidence-backed FAQ drafts land in a
  proposed-knowledge-base repository; patterns needing a process or project decision
  land in a separate improvement-opportunity register for the business owner.
- **Review in an interface.** A single interface shows the end-to-end run and its
  decision records. A content owner accepts, edits or rejects an FAQ recommendation;
  a business owner reviews opportunities.
- **Fail closed on missing evidence.** Where approved material does not support an
  answer, the need is recorded and no answer is drafted.
- **No publication path.** The demonstration has no connection and no write path to
  an active knowledge base. Publication stays a separate, human-triggered action
  outside this process.
- **Evaluate against a labelled key.** Repeatable cases over versioned mock inputs
  make outcomes comparable across runs.

**Not in this change:** connection to an active knowledge-base database; integration
with the customer-care solution's APIs or tools; a live feed of unanswered questions;
SSO; Infosec approvals including the FIRST process; a new EAI with Google Cloud;
direct customer contact; automatic publication; live web search; production
deployment; multilingual handling; executing improvement projects. Deferred to a
later iteration: A2A multi-agent design (this change uses one agent with multiple
tool calls) and a dedicated security-review agent.

## Capabilities

### New Capabilities

- `question-intake`: Accepting mocked unanswered-question and FAQ-catalogue records,
  validating their shape, and screening and sanitizing them for sensitive or
  malicious content before any agent sees them — including the rule that question
  text is data and never instruction.
- `recurring-need-detection`: Grouping semantically similar questions into a single
  recurring need and reporting how often it occurs, deterministically and
  reproducibly across runs.
- `knowledge-assessment`: Deciding, for a recurring need, whether the existing FAQ
  catalogue already answers it, approved reference material supports a new answer,
  the request is out of scope, the evidence is insufficient, or the pattern calls for
  a business improvement — and recording the rationale behind that outcome.
- `faq-recommendation`: Drafting a proposed FAQ answer grounded in cited approved
  passages, refusing to draft when evidence is insufficient, and holding drafts in a
  proposed-knowledge-base repository separate from any active catalogue.
- `improvement-opportunity-register`: Recording recurring patterns that call for a
  process or project decision rather than new documentation, kept apart from FAQ
  recommendations and addressed to the business owner.
- `human-review`: The owner-facing interface over a run — its decision records and
  evidence — where a content owner accepts, edits or rejects a recommendation and a
  business owner reviews opportunities, with no write path to an active knowledge
  base from any of it.
- `run-evaluation`: Running the labelled demonstration corpus end to end and
  reporting measured outcomes against the separate answer key, with stable record
  identities and recorded input and configuration versions so runs can be compared.

### Modified Capabilities

None — this is a greenfield change. `openspec/specs/` is empty.

## Impact

- **New repository structure.** Python implementation, Pydantic models for validation,
  a Google ADK single-agent definition with read-only tools, and the demonstration
  interface. Nothing exists yet beyond `proposal-v3.md` and the `context/` reference
  material.
- **New fixtures.** Versioned mock JSON — 45 FAQ entries across six topics, 59
  labelled unanswered questions, 15 source documents yielding 31 passages — plus an
  answer key stored separately from process input so evaluation cannot leak into it.
- **Dependencies.** Google ADK, Python, Pydantic. No cloud deployment, no customer
  systems, no production data.
- **Process.** OpenSpec governs requirements and change tracking; GitHub holds source
  and version history.
- **Explicitly untouched.** No active knowledge base, no customer-care APIs, no
  customer-facing channel. The demonstration corpus is synthetic and establishes
  nothing about performance on live support data.
