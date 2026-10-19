# Spec Delta

## Purpose

Groups semantically similar unanswered questions into a single recurring need and
reports how often that need occurs, so the process acts on measured recurrence rather
than on anecdote, and so repeated questions do not produce repeated recommendations.

## ADDED Requirements

### Requirement: Similar questions are grouped into one recurring need
The system SHALL group questions that express the same underlying need into a single
recurring need, including questions that differ in wording. Each need SHALL carry a
stable identifier and SHALL list the identifiers of its member questions.

#### Scenario: Rephrased questions join one need
- **WHEN** several admitted questions ask for the same thing in different words
- **THEN** they are grouped into one recurring need whose member list names each contributing question identifier

#### Scenario: Unrelated questions stay separate
- **WHEN** two admitted questions concern different subjects
- **THEN** they are not grouped together, and each is represented by its own need

### Requirement: Recurrence is counted and reported
The system SHALL report, for each recurring need, the number of member questions that
produced it. Downstream assessment and review SHALL have access to that count.

#### Scenario: Count matches membership
- **WHEN** a recurring need is formed from a set of member questions
- **THEN** the reported recurrence equals the number of members in that set

#### Scenario: A single question still forms a need
- **WHEN** an admitted question matches no other question in the batch
- **THEN** it forms a recurring need with a recurrence of one, and is carried into assessment

### Requirement: Grouping is deterministic across runs
Grouping SHALL be deterministic. Re-running the same versioned inputs SHALL produce
the same needs, the same membership, and the same need identifiers. Determinism SHALL
NOT be claimed for model-generated wording elsewhere in the process.

#### Scenario: Repeated run reproduces grouping
- **WHEN** the same versioned mock inputs are processed a second time
- **THEN** the resulting needs, their membership, and their identifiers are identical to the first run

### Requirement: Grouping precedes agent assessment
The system SHALL group questions before submitting needs for agent assessment, so that
a recurring need is assessed once rather than once per member question.

#### Scenario: One assessment per need
- **WHEN** a recurring need is formed from several member questions
- **THEN** assessment is carried out for the need as a whole, not separately for each member question
