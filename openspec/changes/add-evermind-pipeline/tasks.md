# Tasks

## 1. Project scaffolding

- [ ] 1.1 Create the Python package layout (pipeline, agent, interface, evaluation, fixtures) and verify the package imports cleanly from a fresh checkout
- [ ] 1.2 Add dependencies — Google ADK, Pydantic, the test runner, the interface framework — and verify a clean install succeeds and the pinned versions resolve
- [ ] 1.3 Add the test and lint entry points and verify both run green on the empty project

## 2. Data contracts

- [ ] 2.1 Define Pydantic models for question records, FAQ catalogue entries and source passages, and verify round-trip tests accept valid fixtures and reject malformed ones
- [ ] 2.2 Define models for recurring needs, decision records, FAQ recommendations and register entries, with citation required on recommendations, and verify a recommendation cannot be constructed without at least one passage reference
- [ ] 2.3 Define the run-manifest model carrying input and configuration versions, and verify a run record cannot be written without them
- [ ] 2.4 Document the contracts in docs/contracts.md and verify each documented field matches the model definitions

## 3. Demonstration fixtures

- [ ] 3.1 Author the versioned mock FAQ catalogue (45 entries, six topics) and verify it validates against the catalogue model
- [ ] 3.2 Author the versioned unanswered-question fixtures (59 records) including sensitive, malicious and prompt-injection records, and verify all validate against the question model
- [ ] 3.3 Author the approved reference corpus (15 documents, 31 passages) and verify passage extraction yields the expected count and each passage validates
- [ ] 3.4 Author the labelled answer key outside the loaded fixture set and verify a test asserts the process-side loader cannot reach the key path

## 4. Intake and screening

- [ ] 4.1 Implement record validation that excludes malformed records and continues the run, and verify a test with mixed valid and malformed records processes the valid ones and reports the rest
- [ ] 4.2 Implement sensitive-content screening that excludes and flags records, and verify the excluded content appears in no log, draft or register entry
- [ ] 4.3 Implement malicious-content screening that excludes and flags records, and verify the payload appears in no log or repository
- [ ] 4.4 Implement the untrusted-data frame that passes question text inside explicit delimiters, and verify an injection-bearing record leaves process instructions and decision outcomes unchanged
- [ ] 4.5 Document the screening gate and its false-positive handling in docs/screening.md and verify the documented behaviour matches the tests

## 5. Recurring-need detection

- [ ] 5.1 Implement deterministic grouping with a fixed threshold and total tie-breaking, and verify rephrased questions group together while unrelated ones do not
- [ ] 5.2 Implement content-derived need identifiers, and verify re-running the same fixtures reproduces identical needs, membership and identifiers
- [ ] 5.3 Implement recurrence counting including the single-question case, and verify the reported count equals group membership and a lone question yields a need with recurrence one
- [ ] 5.4 Tune the similarity threshold against the labelled corpus and verify the grouping-accuracy case reports the measured result

## 6. Agent and retrieval tools

- [ ] 6.1 Define the single Google ADK agent with its fixed instruction frame, and verify its configuration holds the process instructions separately from any record data
- [ ] 6.2 Implement the read-only catalogue-lookup tool and verify a test asserts it exposes no mutating operation
- [ ] 6.3 Implement passage-level retrieval over the approved corpus and verify it returns passages rather than whole documents, with references that resolve
- [ ] 6.4 Verify by test that the codebase contains no knowledge-base client, publication function or write credential

## 7. Knowledge assessment

- [ ] 7.1 Implement existing-catalogue matching ahead of any drafting, and verify a rephrased known question yields the existing-FAQ outcome naming the matched entry
- [ ] 7.2 Implement the out-of-scope outcome with recorded reason, and verify no answer text is produced for such a need
- [ ] 7.3 Implement the insufficient-evidence outcome, and verify a need with no supporting passage produces no draft and stays visible to the reviewer
- [ ] 7.4 Implement business-improvement-opportunity classification, and verify a process-problem need produces no FAQ recommendation
- [ ] 7.5 Implement decision records carrying recurrence, reason and evidence references, and verify every need in a run has exactly one outcome with a complete record
- [ ] 7.6 Implement fail-closed handling for unreadable input or configuration, and verify the affected need receives no outcome and the failure is reported
- [ ] 7.7 Document the five outcomes and their decision rules in docs/outcomes.md and verify the documented rules match the implemented behaviour

## 8. Recommendations and register

- [ ] 8.1 Implement FAQ drafting grounded in cited passages, and verify every produced draft carries at least one resolving passage reference
- [ ] 8.2 Implement the proposed-knowledge-base store, and verify a run writes drafts there and leaves the input catalogue byte-identical
- [ ] 8.3 Implement draft traceability to need, recurrence and citations, and verify a stored draft resolves back to its originating questions
- [ ] 8.4 Implement the improvement-opportunity register as a separate store, and verify no entry appears among recommendations and no recommendation appears in the register
- [ ] 8.5 Verify by test that neither store contains screened sensitive or malicious content after a full run

## 9. Review interface

- [ ] 9.1 Implement the run view showing needs, outcomes, recommendations and opportunities, and verify a completed run renders all four
- [ ] 9.2 Implement the decision-record view showing recurrence, outcome, reason and evidence, and verify each element is present for every outcome type
- [ ] 9.3 Implement accept, edit and reject on recommendations with the original draft preserved on edit, and verify each action is recorded and the original text survives an edit
- [ ] 9.4 Implement the business-owner register view, and verify each entry shows its need, recurrence and rationale
- [ ] 9.5 Verify by test that no interface action writes to the input catalogue or any active knowledge base, and that no customer-facing output is produced
- [ ] 9.6 Document how to run the interface against a completed run in docs/demo.md and verify the documented steps work from a clean checkout

## 10. Evaluation harness

- [ ] 10.1 Implement the harness comparing assigned outcomes to the answer key over the full batch, and verify the report covers every need and is refused if the batch is partial
- [ ] 10.2 Implement run manifests recording input and configuration versions, and verify a result can be attributed to the fixtures that produced it
- [ ] 10.3 Implement the repeatability case comparing outcomes, identities and groupings while ignoring generated wording, and verify a re-run passes and a wording difference does not fail it
- [ ] 10.4 Implement the declared evaluation cases — supported need, insufficient evidence, existing answer, out of scope, sensitive input, malicious input, prompt injection, business opportunity, human decision boundary — and verify each is exercised and reported
- [ ] 10.5 Implement the no-write regression case, and verify it confirms no run wrote to an active knowledge base or the input catalogue
- [ ] 10.6 Add the synthetic-corpus limitation statement to evaluation output and verify it appears in every report

## 11. End-to-end verification

- [ ] 11.1 Run the full pipeline over the complete fixture set and verify every need receives exactly one outcome and the run manifest is complete
- [ ] 11.2 Run the evaluation suite end to end and verify every declared case reports a result
- [ ] 11.3 Re-run the pipeline on identical fixtures and verify outcomes, identifiers and groupings match the previous run
- [ ] 11.4 Walk the demonstration script from a clean checkout and verify docs/demo.md reproduces the presented run without undocumented steps

## Workflow follow-up

- Present the demonstration to the Capstone Committee with the deck.
- Archive the change once the committee's review requirements are satisfied, and verify the archived result.
