# Spec Delta

## Purpose

Accepts the mocked unanswered-question and FAQ-catalogue records that feed a run,
validates their shape, and screens them for sensitive or malicious content before any
agent processes them, so that unsafe material never reaches logs, drafts or registers.

## ADDED Requirements

### Requirement: Input records are validated before processing
The system SHALL validate every unanswered-question and FAQ-catalogue record against
its declared schema before the record enters a run. A record that fails validation
SHALL be excluded from the run and reported with its identifier and the reason, and
SHALL NOT cause the run to abort.

#### Scenario: Well-formed record is accepted
- **WHEN** a question record carries every required field with values of the declared types
- **THEN** the record is admitted to the run and retains the identifier given in the source fixture

#### Scenario: Malformed record is excluded, run continues
- **WHEN** a record is missing a required field or carries a value of the wrong type
- **THEN** the record is excluded from the run, its identifier and the validation failure are reported, and the remaining records are still processed

### Requirement: Sensitive content is screened out before agent processing
The system SHALL screen each record for personal information and company-confidential
content before any agent call. A record found to carry such content SHALL be excluded
from agent processing and flagged for human review. Only the record identifier and the
screening outcome SHALL be recorded; the offending content SHALL NOT appear in logs,
FAQ recommendations, or the improvement-opportunity register.

#### Scenario: Record carrying personal information is withheld
- **WHEN** a question record contains personal information such as a customer name, account number or contact detail
- **THEN** the record is excluded from agent processing, flagged for human review, and neither the log nor any downstream repository contains the personal information

#### Scenario: Screening outcome is traceable without exposing content
- **WHEN** a record has been excluded by screening
- **THEN** the run record shows that record's identifier and the screening outcome, and shows no excerpt of the screened content

### Requirement: Malicious content is screened out before agent processing
The system SHALL screen each record for malicious payloads, including executable code
and attack strings. A record found to carry such content SHALL be excluded from agent
processing and flagged for human review, and the payload SHALL NOT be written to logs
or either candidate repository.

#### Scenario: Record carrying executable payload is withheld
- **WHEN** a question record contains executable code or a recognised attack string
- **THEN** the record is excluded from agent processing, flagged for human review, and the payload appears in no log or repository

### Requirement: Question text is data and never instruction
The system SHALL treat the text of every question record as untrusted data. Question
text SHALL NOT alter the agent's process instructions, change a decision outcome, or
authorize any action.

#### Scenario: Question attempts to instruct the agent
- **WHEN** a question record contains text directing the agent to ignore its instructions, publish content, or change its decision
- **THEN** the embedded direction has no effect on the process instructions or on any decision outcome, and the record is assessed on its content as an ordinary customer question

#### Scenario: Injection attempt is visible to the reviewer
- **WHEN** a question record is found to contain an instruction-like directive
- **THEN** the run record notes the attempt against the record identifier so a human reviewer can see it was detected and neutralised
