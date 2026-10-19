# Spec Delta

## Purpose

Decides, for each recurring need, whether existing approved knowledge already answers
it, whether approved material supports a new answer, or whether the pattern calls for
a business decision instead, and records the rationale behind whichever outcome it
reaches.

## ADDED Requirements

### Requirement: Every recurring need receives exactly one outcome
The system SHALL assign each recurring need exactly one outcome from: FAQ
recommendation, existing FAQ, out of scope, insufficient evidence, or business
improvement opportunity. No need SHALL be left without an outcome, and no need SHALL
carry more than one.

#### Scenario: Each need carries one outcome
- **WHEN** a run completes over a batch of recurring needs
- **THEN** every need in that run shows exactly one of the five outcomes

### Requirement: Existing catalogue is checked before new content is proposed
The system SHALL compare each recurring need against the FAQ catalogue before
proposing new content. Where the catalogue already answers the need, the outcome SHALL
be existing FAQ and the matching catalogue entry SHALL be identified.

#### Scenario: Rephrased question matches an existing entry
- **WHEN** a recurring need restates something the FAQ catalogue already answers
- **THEN** the outcome is existing FAQ, the matching entry is named, and no new answer is drafted

### Requirement: Needs the catalogue should not answer are marked out of scope
Where a recurring need is one the FAQ catalogue should not answer, the system SHALL
record the outcome as out of scope together with the reason, and SHALL NOT draft an
answer.

#### Scenario: Out-of-scope request is recorded with its reason
- **WHEN** a recurring need asks for something the catalogue should not address
- **THEN** the outcome is out of scope, the reason is recorded, and no answer text is produced

### Requirement: Unsupported needs are recorded rather than answered
Where a recurring need is genuine but the approved reference material does not support
an answer, the system SHALL record the outcome as insufficient evidence and SHALL NOT
draft an answer.

#### Scenario: Approved material does not cover the need
- **WHEN** no approved passage supports an answer to a recurring need
- **THEN** the outcome is insufficient evidence, the need remains visible to the reviewer, and no answer is drafted

### Requirement: Patterns calling for business change are routed as opportunities
Where a recurring need indicates a business process or project problem rather than a
documentation gap, the system SHALL record the outcome as a business improvement
opportunity rather than drafting an FAQ.

#### Scenario: Recurring need points to a process problem
- **WHEN** a recurring need reflects an underlying process or product problem that new documentation would not resolve
- **THEN** the outcome is business improvement opportunity, and no FAQ recommendation is produced for that need

### Requirement: Each outcome records its rationale and evidence
The system SHALL record, for each outcome, the recurrence of the need, the reason for
the outcome, and references to the evidence considered. The record SHALL be sufficient
for a reviewer to understand why the outcome was reached without re-running the
process.

#### Scenario: Reviewer can trace an outcome
- **WHEN** a reviewer opens the decision record for any recurring need
- **THEN** the record shows the recurrence, the stated reason, and references to the catalogue entries or approved passages considered

### Requirement: Assessment reads approved sources only
Assessment SHALL draw on the mock FAQ catalogue and the approved reference corpus
only. The system SHALL NOT consult live web search or any source outside the approved
demonstration material, and SHALL use read-only access to those sources.

#### Scenario: No external source is consulted
- **WHEN** a recurring need is assessed
- **THEN** only the mock catalogue and approved corpus are read, and no external or live source is contacted

### Requirement: Assessment fails closed when required data is missing
Where required input or configuration is missing or unreadable, the system SHALL stop
without producing an outcome for the affected need and SHALL report the failure. It
SHALL NOT substitute a default outcome or an unsupported answer.

#### Scenario: Reference corpus is unavailable
- **WHEN** the approved reference material cannot be read during assessment
- **THEN** the affected need receives no outcome, the failure is reported, and no answer is drafted
