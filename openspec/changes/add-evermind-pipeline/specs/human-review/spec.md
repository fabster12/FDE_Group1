# Spec Delta

## Purpose

Gives the content owner and business owner a single interface over a run — its
decision records and the evidence behind them — where recommendations are accepted,
edited or rejected, while no action taken there can reach an active knowledge base.

## ADDED Requirements

### Requirement: The interface shows the end-to-end run
The interface SHALL present a completed run: the recurring needs identified, the
outcome assigned to each, the FAQ recommendations produced, and the improvement
opportunities recorded.

#### Scenario: Owner views a completed run
- **WHEN** a content owner opens a completed run in the interface
- **THEN** the run's recurring needs, their outcomes, the FAQ recommendations and the improvement opportunities are all visible

### Requirement: Every decision record shows its rationale and evidence
For each recurring need the interface SHALL show the recurrence, the outcome, the
stated reason, and the catalogue entries or approved passages on which the outcome
rests.

#### Scenario: Owner inspects why an outcome was reached
- **WHEN** an owner opens the decision record for a recurring need
- **THEN** the recurrence, the outcome, the reason, and the supporting evidence references are shown together

### Requirement: The content owner can accept, edit or reject a recommendation
The interface SHALL let a content owner accept, edit or reject each FAQ
recommendation. The system SHALL record which action was taken, and an edit SHALL
preserve the original draft alongside the edited text.

#### Scenario: Owner accepts a recommendation
- **WHEN** a content owner accepts an FAQ recommendation
- **THEN** the acceptance is recorded against that recommendation

#### Scenario: Owner edits a recommendation
- **WHEN** a content owner edits the text of an FAQ recommendation
- **THEN** the edited text and the original draft are both retained, and the edit is recorded

#### Scenario: Owner rejects a recommendation
- **WHEN** a content owner rejects an FAQ recommendation
- **THEN** the rejection is recorded and the recommendation is not presented as approved content

### Requirement: The business owner can review improvement opportunities
The interface SHALL let a business owner review the improvement-opportunity register
with each entry's originating need, recurrence and rationale.

#### Scenario: Business owner reviews the register
- **WHEN** a business owner opens the improvement-opportunity register in the interface
- **THEN** each entry is shown with its originating need, recurrence and rationale

### Requirement: Review never writes to an active knowledge base
No action available in the interface SHALL write to, publish to, or otherwise modify
an active knowledge base or the FAQ catalogue used as process input. The system SHALL
have no connection or write path to an active knowledge base.

#### Scenario: Accepting a recommendation changes no catalogue
- **WHEN** a content owner accepts an FAQ recommendation
- **THEN** the acceptance is recorded, the FAQ catalogue used as input is unchanged, and nothing is published

#### Scenario: No write path exists to exercise
- **WHEN** the demonstration is inspected for routes to an active knowledge base
- **THEN** no connection or write path to one is present

### Requirement: Publication remains a separate human-triggered action
Publishing approved content SHALL be an activity outside this process, triggered by a
human. The system SHALL NOT publish automatically on acceptance or on any other event.

#### Scenario: Acceptance does not trigger publication
- **WHEN** an FAQ recommendation is accepted in the interface
- **THEN** no publication occurs, automatically or otherwise, and any later publication is a separate human-triggered activity outside this process

### Requirement: The interface does not contact customers
The system SHALL NOT answer customers directly or contact them through any channel.

#### Scenario: No customer-facing output
- **WHEN** a run completes and recommendations are reviewed
- **THEN** no message, answer or notification is sent to any customer
