# Design

## Context

Greenfield. The repository holds `proposal-v3.md` and course reference material under
`context/`; there is no code. See proposal.md — Why for the motivation.

Three constraints shape everything below. The platform is fixed by the capstone:
Google ADK for the agent, Python, Pydantic for validation, OpenSpec for change
tracking, GitHub for source. The data is mocked: 45 FAQ entries across six topics, 59
labelled unanswered questions, 15 source documents yielding 31 passages, plus an
answer key held apart from process input. And the process is a batch demonstration
with a human at the end — nothing it produces reaches a customer or an active
knowledge base.

Three people build it (Yasin Durgun leading, Fabian William Bakker on the repository,
Siva Prasada Reddy Sogalapalli and Fabian on evaluation), so the work has to split
into parts that can progress in parallel against agreed contracts.

## Goals / Non-Goals

**Goals:**

- One agent, several read-only tool calls — the A2A design is explicitly deferred, so
  the architecture must not presume it.
- A hard structural boundary between the process and anything writable: the absence
  of a knowledge-base write path should be evident from the code's shape, not
  maintained by discipline.
- Deterministic work (validation, grouping, catalogue matching) separated from
  model-dependent work (interpretation, drafting), so repeatability claims hold where
  the specs make them and are not claimed where they cannot be.
- Contracts — Pydantic models — fixed early, so interface, pipeline and evaluation can
  be built concurrently by three people.

**Non-Goals:**

- Performance targets. This is a batch run; no live response window applies.
- Durable storage design. Run artifacts are files; no database is introduced.
- Authentication or multi-user concerns in the interface. Reviewer identity is not
  modelled.
- Abstraction over the model provider. ADK is a given, not a decision to hedge.

## Decisions

### Pipeline stages as pure functions over Pydantic models

Each stage — intake, grouping, assessment, drafting, registering — takes validated
models and returns validated models. The run is their composition.

*Why:* every stage boundary becomes independently testable, which is what the
evaluation capability needs; and the three workstreams can agree on model definitions
and then build against them without blocking each other.

*Alternative considered:* a single agent orchestration loop performing all steps
through tool calls. Rejected — it would put deterministic grouping inside the model's
control, breaking the determinism `recurring-need-detection` requires, and would make
per-stage evaluation impossible.

### Deterministic grouping, not model-driven clustering

Questions are grouped by a deterministic similarity procedure with a fixed threshold,
computed from embeddings or lexical features, with the clustering reproducible and the
tie-breaking total.

*Why:* `recurring-need-detection` requires identical needs, membership and identifiers
across runs. A model asked to cluster gives neither stable membership nor stable
identifiers.

*Alternative considered:* asking the agent to group questions. Rejected for
determinism; also costlier, since grouping is the step that reduces the agent's
workload in the first place.

### Content-derived identifiers

A need's identifier derives from a canonical form of its sorted member question
identifiers; recommendation and register-entry identifiers derive from the need they
came from. No counters, no timestamps, no run-scoped sequence numbers.

*Why:* `run-evaluation` requires identities stable across re-runs so results can be
compared. Content derivation gives this without a registry to maintain.

*Trade-off:* adding one question to a group changes the need's identifier. For a
versioned demonstration corpus this is acceptable and arguably correct — a different
group of questions is a different need.

### Screening before the agent, as a gate rather than a filter

Intake screening runs before any agent call and excludes the record entirely. The
agent's input is built from admitted records only; excluded records appear downstream
solely as an identifier plus an outcome.

*Why:* R3 and R4. A filter that redacts and passes content through leaves the
redaction one bug away from a leak into logs or drafts. A gate that excludes makes the
unsafe content structurally unable to reach the agent, the logs, or either repository.

*Trade-off:* a false positive loses a legitimate question from the run. Mitigated by
flagging excluded records for human review rather than discarding them silently — the
reviewer sees that a record was withheld and why.

### Question text passed as delimited data with a fixed instruction frame

Process instructions live in the agent's own configuration. Question text enters only
inside explicit data delimiters, never concatenated into the instruction text.

*Why:* R4 — question text must not be able to change the process instructions. The
structural separation does the work; prompt wording alone would not.

*Trade-off:* no framing fully eliminates injection risk. The real protection is that
the agent's tools are read-only and the system has no write path at all — an injected
instruction has nothing to reach for. This is why the next decision is structural
rather than procedural.

### Read-only tools, and no write path in the codebase at all

The agent's tools retrieve: catalogue lookup, passage retrieval. None mutates. Beyond
the agent, the codebase contains no knowledge-base client, no publication function,
and no credentials for one.

*Why:* R5, and `human-review`'s requirement that no write path exists to exercise. A
capability that is absent cannot be invoked by a bug or by an injected instruction. It
also makes the no-write regression test straightforward: the test asserts an absence
rather than the correctness of a guard.

*Alternative considered:* a write path behind a disabled feature flag, ready for a
later iteration. Rejected — a flag is a thing to be flipped by accident, and the
proposal places publication outside this process entirely.

### Fail closed on missing evidence and on missing data

Where approved passages do not support an answer, the outcome is insufficient evidence
and no draft is produced. Where required input or configuration cannot be read, the
affected need gets no outcome and the failure is reported; no default outcome is
substituted.

*Why:* R1, and `knowledge-assessment`'s fail-closed requirement. The costly failure
here is a confident answer with nothing behind it, not a missing answer.

### Evidence citation as a structural requirement of the draft model

The recommendation model cannot be constructed without at least one passage reference;
validation enforces it.

*Why:* R1. Making the citation part of the type means an uncited draft cannot exist to
be reviewed, rather than being caught by a check someone might skip.

### Retrieval over passages, not documents

The 15 source documents are split into their 31 passages; retrieval returns passages.

*Why:* cost and focus — the agent reads what is relevant rather than whole documents —
and citation granularity: a passage reference points a reviewer at the sentence that
supports the claim.

### Separate stores for drafts and opportunities

The proposed-knowledge-base repository and the improvement-opportunity register are
distinct stores with distinct models, both separate from the input FAQ catalogue.

*Why:* `faq-recommendation` and `improvement-opportunity-register` require the
separation; the two have different readers (content owner, business owner) and
different lifecycles. Distinct models also prevent an opportunity being surfaced by
mistake as publishable content.

### Answer key read only by the evaluation harness

The key lives outside the fixtures the process loads, and only the harness opens it,
after the run.

*Why:* `run-evaluation` requires the process not to read it. Keeping it outside the
loaded fixture set makes leakage a visible change to the code rather than an oversight.

### Run artifacts as versioned files

A run writes its decision records, drafts, register entries and the input and
configuration versions to files under a run directory.

*Why:* enough for comparison across runs and for the interface to read, with no
database to stand up. The demonstration is local and batch.

## Risks / Trade-offs

- **Deterministic grouping mis-groups where a model would not** → fixed threshold
  tuned against the labelled corpus; grouping quality is itself an evaluation case, so
  a bad threshold shows up as measured error rather than as a surprise in the demo.
- **Determinism is claimed too broadly** → repeatability is asserted for outcomes,
  identities and groupings only; `run-evaluation` states explicitly that generated
  wording may vary, and the harness does not compare wording.
- **Screening excludes legitimate questions (false positives)** → excluded records are
  flagged for human review with their identifier and outcome, so a reviewer sees the
  loss; the evaluation suite covers sensitive and malicious cases so the rate is
  measured, not assumed.
- **An assessment outcome is plausible but wrong** → every outcome carries its
  rationale and evidence references, and the labelled key measures outcome accuracy
  over the full batch rather than on a chosen example.
- **Synthetic corpus flatters the result** → evaluation output states the corpus is
  synthetic and establishes nothing about live support data; this is a requirement in
  `run-evaluation`, not a caveat left to the presenter.
- **Prompt injection despite framing** → defence rests on read-only tools and the
  absent write path, not on framing alone; the injection case is an evaluation case
  and the no-write boundary is a regression test.
- **Content-derived identifiers shift when fixtures change** → fixtures are versioned
  and runs record the version they used, so a comparison across differing fixture
  versions is visibly that.

## Migration Plan

None. Greenfield demonstration with no existing system, no data to migrate and no
production deployment. Rollback is reverting the change in version control.
