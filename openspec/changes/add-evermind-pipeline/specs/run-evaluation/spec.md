# Spec Delta

## Purpose

Runs the labelled demonstration corpus end to end and reports measured outcomes
against a separately held answer key, with stable record identities and recorded input
versions, so that runs can be compared and improvement can be shown with facts.

## ADDED Requirements

### Requirement: The answer key is held apart from process input
The labelled answer key SHALL be stored separately from the records the process reads.
The process SHALL NOT read the answer key during a run; only evaluation SHALL read it,
after the run has completed.

#### Scenario: Process cannot see the key
- **WHEN** a run is executed over the demonstration corpus
- **THEN** the process reads only the question records, the FAQ catalogue and the approved corpus, and does not read the answer key

### Requirement: Evaluation reports measured outcomes against the key
Evaluation SHALL compare each need's assigned outcome with the expected outcome in the
answer key and report the results over the full batch, not over a selected subset.

#### Scenario: Full batch is reported
- **WHEN** evaluation runs over the demonstration corpus
- **THEN** the report covers every need in the batch and states where assigned outcomes matched expected ones and where they did not

### Requirement: Record identities are stable across runs
Needs, recommendations and register entries SHALL carry identifiers that remain stable
when the same versioned inputs are re-run, so results can be compared run to run.

#### Scenario: Re-run preserves identities
- **WHEN** the same versioned inputs are processed a second time
- **THEN** the needs, recommendations and register entries carry the same identifiers as in the first run

### Requirement: Runs record their input and configuration versions
Each run SHALL record the versions of the mock inputs and of the process configuration
used, so a result can be tied to what produced it.

#### Scenario: Result is attributable to its inputs
- **WHEN** a run completes
- **THEN** the run record names the versions of the mock inputs and process configuration used

### Requirement: Repeatability claims exclude model wording
Repeatability SHALL be claimed for outcomes, identities and groupings. The system
SHALL NOT claim that model-generated answer wording is identical between runs.

#### Scenario: Wording may vary while outcomes hold
- **WHEN** the same versioned inputs are re-run
- **THEN** the outcomes, identifiers and groupings match the earlier run, while any difference in generated answer wording is not treated as a failure

### Requirement: The demonstration covers the declared cases
Evaluation SHALL exercise, over the labelled corpus, a supported FAQ need, an
insufficient-evidence need, an existing-answer need, an out-of-scope need, sensitive
and malicious inputs, a prompt-injection attempt, a business improvement opportunity,
a repeated run, and the human decision and publication boundary.

#### Scenario: Declared cases are all exercised
- **WHEN** the evaluation suite is run
- **THEN** each of the declared cases is exercised and its result reported

#### Scenario: No-write boundary is checked as a regression
- **WHEN** the evaluation suite runs the publication-boundary case
- **THEN** it confirms that no run wrote to an active knowledge base or to the input FAQ catalogue

### Requirement: Reported results state the limits of the corpus
Evaluation output SHALL state that the corpus is synthetic and that results do not
establish performance on live support data.

#### Scenario: Limits accompany the results
- **WHEN** evaluation results are reported
- **THEN** the report states that the corpus is synthetic and that the results say nothing about live support data
