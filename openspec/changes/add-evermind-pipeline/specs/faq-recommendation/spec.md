# Spec Delta

## Purpose

Produces proposed FAQ answers that are grounded in cited approved passages, refuses to
draft where the evidence does not support an answer, and holds every draft in a
proposed-knowledge-base repository that is separate from any active catalogue.

## ADDED Requirements

### Requirement: Every proposed answer cites approved evidence
The system SHALL attach to each FAQ recommendation the approved passages that support
it. A recommendation SHALL NOT be produced without at least one supporting passage
reference.

#### Scenario: Supported need yields a cited draft
- **WHEN** approved passages support an answer to a recurring need
- **THEN** an FAQ recommendation is produced carrying references to the passages that support it

#### Scenario: Draft without evidence is not produced
- **WHEN** no approved passage supports an answer
- **THEN** no FAQ recommendation is produced for that need

### Requirement: Proposed answers stay within their evidence
A proposed answer SHALL assert only what its cited passages support. The system SHALL
NOT produce an answer containing claims absent from the cited material.

#### Scenario: Answer confined to cited material
- **WHEN** a proposed answer is produced from a set of cited passages
- **THEN** each claim it makes is traceable to one of those passages

#### Scenario: Partially supported need is not over-claimed
- **WHEN** approved passages cover only part of a recurring need
- **THEN** the proposed answer addresses only the supported part, or the need is recorded as insufficient evidence, and the unsupported part is not asserted

### Requirement: Recommendations are held in a separate proposed repository
The system SHALL write FAQ recommendations to a proposed-knowledge-base repository
that is distinct from the FAQ catalogue used as process input. A recommendation SHALL
NOT be written into that catalogue.

#### Scenario: Draft lands in the proposed repository
- **WHEN** an FAQ recommendation is produced
- **THEN** it appears in the proposed-knowledge-base repository and the FAQ catalogue used as input is unchanged

### Requirement: Recommendations carry traceability to their origin
Each FAQ recommendation SHALL identify the recurring need it answers, that need's
recurrence, and the cited passages, so a reviewer can trace the draft back to the
questions and evidence that produced it.

#### Scenario: Reviewer traces a draft to its questions
- **WHEN** a reviewer opens an FAQ recommendation
- **THEN** it shows the originating need identifier, the recurrence, and the cited approved passages

### Requirement: Recommendations exclude screened content
An FAQ recommendation SHALL NOT contain personal information, company-confidential
content, or malicious payloads from any source record.

#### Scenario: Screened content never reaches a draft
- **WHEN** records carrying sensitive or malicious content were excluded during intake
- **THEN** no FAQ recommendation in the run contains that content
