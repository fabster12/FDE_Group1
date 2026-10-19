# Spec Delta

## Purpose

Records recurring patterns that call for a business process or project decision rather
than new documentation, keeping them apart from FAQ recommendations so the business
owner sees improvement opportunities without them being mistaken for content gaps.

## ADDED Requirements

### Requirement: Opportunities are held separately from FAQ recommendations
The system SHALL record business improvement opportunities in a register distinct from
the proposed-knowledge-base repository. An opportunity SHALL NOT appear as an FAQ
recommendation, and an FAQ recommendation SHALL NOT appear in the register.

#### Scenario: Opportunity is routed to its own register
- **WHEN** a recurring need is assessed as a business improvement opportunity
- **THEN** it is recorded in the improvement-opportunity register and does not appear among the FAQ recommendations

### Requirement: Each opportunity states its evidence and rationale
Each register entry SHALL record the recurring need it arose from, that need's
recurrence, and the reason the pattern calls for a business decision rather than a new
FAQ.

#### Scenario: Business owner reads an entry
- **WHEN** a business owner opens a register entry
- **THEN** it shows the originating need, the recurrence, and the stated reason a business decision is called for rather than documentation

### Requirement: Register entries exclude screened content
A register entry SHALL NOT contain personal information, company-confidential content,
or malicious payloads from any source record.

#### Scenario: Screened content never reaches the register
- **WHEN** records carrying sensitive or malicious content were excluded during intake
- **THEN** no register entry in the run contains that content

### Requirement: The register records opportunities without executing them
The register SHALL capture an opportunity for a business owner to consider. The system
SHALL NOT initiate, schedule or execute any improvement project, and SHALL NOT treat a
register entry as a decision.

#### Scenario: Entry is a proposal, not an action
- **WHEN** an opportunity is recorded in the register
- **THEN** no project work is initiated or scheduled, and the entry remains an item awaiting the business owner's consideration
