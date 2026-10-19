# Proposal: EverMind

**Change:** `add-evermind-pipeline`  
**Version:** 3.0 · supersedes `proposal-v2.md` (v2.0)  
**Gate:** Phase 1 — Define and Plan  
**Group:** 1, Netherlands cohort (three members)  
**Type:** Greenfield demonstration — mocked data and a new end-to-end interface  
**Date:** 19 October 2026 · **Status:** Approved by Capstone Committee

---

## Problem

A support knowledge base can only answer what has been documented. When it
has no answer, customers are handed over to call-center agents. Customers
spend minutes explaining the same need, while agents and the company spend
valuable time and effort resolving repeated questions one at a time. Those
questions are rarely captured as a reliable signal about what the knowledge
base or the business should improve.

**Who feels it.** Customers wait for help. Call-center agents repeatedly
handle questions that may have a common answer or a common underlying cause.
The knowledge-base content owner—the internal customer of this work—has no
reliable view of recurring needs and must rely on anecdotes when deciding
what to update. Business owners may also miss patterns that call for a wider
process or project improvement rather than a new FAQ.

**Get It Right the First Time: The 1-10-100 Rule.** QDM explains the cost of
quality in terms of doing work right the first time: if it costs $1 to do the
work correctly now, correcting it later may cost $10, and replacing a
customer lost because of an error may cost $100. Repeatedly answering the
same question by hand spends time without creating a lasting improvement.
Identifying the pattern and acting on it can improve the customer experience
while reducing repeated effort. QDM is a continuous-improvement philosophy with aims similar to Lean Six Sigma: focus on customer-defined quality, use facts to understand causes, reduce unnecessary work, and measure whether changes improve outcomes.

**Opportunity statement.**

| | |
| --- | --- |
| **Current state** | Recurring unanswered questions are handled one at a time by call-center agents. Patterns are not consistently captured, and the content owner has limited evidence for deciding whether the FAQ or a business process needs attention. |
| **Impact** | Customers and agents spend time repeating work; useful knowledge remains undocumented; business improvement opportunities can be missed. |
| **Desired state** | A simple end-to-end demonstration turns recurring, mocked questions into evidence-backed FAQ recommendations or business improvement opportunities for an owner to review and decide on. |

**Why an AI-native approach.** Design around the work and use each capability
where it fits: ordinary software groups repeated records and validates
inputs; a single Google ADK agent uses multiple read-only tool calls to
interpret questions, check approved knowledge and prepare grounded
recommendations; a human makes the business decision and triggers any later
publication. AI is used for language-heavy judgement, while evidence,
validation and human ownership keep the process bounded.

## The business decision process

EverMind is a business decision-support process demonstrated through a
simple interface. It identifies recurring needs, checks existing approved
knowledge and prepares recommendations for a human owner. It does not
publish to the knowledge base or contact customers.

**Demonstration inputs**

| Input | What it contains |
| --- | --- |
| Mock unanswered questions | JSON records representing questions that would otherwise be handed to a call-center agent. They serve as voice-of-the-customer evidence in the demonstration. |
| Mock FAQ catalogue | JSON records representing existing approved answers to check before proposing new content. |
| Approved reference material | Trusted source passages used to support FAQ recommendations. |

**Three steps, using QDM language**

1. **Identify recurring needs.** Group similar unanswered questions and
   measure how often they occur. Treat recurrence as a signal to investigate
   the underlying need, using facts from the demonstration data rather than
   assumptions.
2. **Check the existing knowledge and cause.** Compare the need with the
   current FAQ catalogue and approved reference material. Determine whether
   the existing knowledge answers it, whether a supported FAQ could help, or
   whether the pattern points to a broader business improvement opportunity.
3. **Prepare a reviewable recommendation.** Provide the owner with the
   recurrence, a clear reason, and relevant evidence. Keep FAQ recommendations
   in a proposed-knowledge-base repository and business improvement
   opportunities in a separate register for the business owner.

The interface shows the end-to-end run and its decision records. A content
owner can accept, edit or reject an FAQ recommendation. A business owner can
review a proposed improvement opportunity. Any later knowledge-base update
is a separate, human-triggered activity outside this process. No automatic
publishing occurs.

## Decision outcomes

Each recurring need is represented in the demonstration with its decision
and rationale. The process can identify:

| Outcome | What it means |
| --- | --- |
| **FAQ recommendation** | A recurring need is not already answered and approved reference material supports a proposed answer for human review. |
| **Existing FAQ** | The current catalogue already answers the need; the matching entry is shown. |
| **Out of scope** | The catalogue should not answer the request; the reason is recorded. |
| **Insufficient evidence** | The need is real, but the approved material does not support an answer, so no answer is drafted. |
| **Business improvement opportunity** | The recurring pattern appears to require a business process or project decision rather than a new FAQ; it is recorded separately for the business owner. |

Human approval remains mandatory for FAQ recommendations. Acceptance,
editing or rejection in the demonstration does not update an active knowledge
base. Project execution is outside the process; the register captures an
opportunity for a business owner to consider.

## Scope

**In.** A simple interface demonstrating the end-to-end process; mocked JSON
for the existing FAQ catalogue and unanswered questions; approved reference
material; identifying and grouping recurring needs; checking existing
knowledge; preparing evidence-backed FAQ recommendations; displaying the
process rationale; a proposed-knowledge-base repository; a separate business
improvement-opportunity register; and human review of recommendations in the
demonstration.

**Out of scope for now.** Connection to an active knowledge-base database;
integration with existing APIs or tools of the customer-care solution; a live
feed or import from the existing unanswered-customer-questions source; SSO
integration; formal Infosec approvals, including the FIRST process and related
approvals; creating a new EAI (enterprise application integration) with Google
Cloud; direct customer answers or contact; automatic or direct publication;
live web search; production deployment; multilingual handling; and executing
business improvement projects.

**Future considerations, out of scope for this iteration:**

- A2A design. For this demonstration, use one agent with multiple tool calls.
- A dedicated security agent for end-to-end security review and penetration
testing, or a future connection with the Infosec team and its agent or skill.

## Demonstration data

The FAQ catalogue and unanswered questions are mocked JSON, and the
reference material is a local, approved demonstration corpus. The existing
seeded fixtures contain 45 FAQ entries across six topics, 59 labelled
unanswered questions, and 15 source documents with 31 passages. The answer
key is kept separate from the process input so evaluation can compare
outcomes against expected results.

## Risks and safeguards

| # | Risk | Safeguard | Evidence |
| --- | --- | --- | --- |
| R1 | A recommendation claims more than the approved evidence supports. | Require source evidence for a proposed answer. If the evidence is missing or insufficient, record that outcome and do not draft an answer. | Supported and unsupported cases |
| R2 | Repeated questions create many similar recommendations. | Group recurring needs and check the current FAQ catalogue before proposing new content. | Recurrence and duplicate cases |
| R3 | Personal information, company secrets, or malicious requests or code pass through the agent into logs or a candidate knowledge base. | Screen and sanitize inputs before processing. Do not place sensitive or malicious content in logs, FAQ recommendations, or the business-opportunity register. Exclude and flag affected records for human review; log only safe record identifiers and outcomes. | Adversarial and sensitive-input cases |
| R4 | A logged question tries to instruct the agent. | Treat question text as untrusted data. It cannot change the process instructions or authorize actions. | Prompt-injection case |
| R5 | A recommendation is published without approval. | The demonstration has no connection or write path to an active knowledge base. Publication remains a separate human-triggered action. | No-write regression case |
| R6 | A question that calls for a business change is incorrectly treated as an FAQ gap. | Show business improvement opportunities in a separate register with their rationale for the business owner. | Business-opportunity case |

## How we will demonstrate it works

Use the labelled mock data and repeatable evaluation cases to show the
process end to end. QDM emphasizes facts, measurement and continuous
improvement; the demonstration therefore reports outcomes that can be checked
and compared across runs.

| Case | Demonstrates |
| --- | --- |
| Supported FAQ need | A recurring, supported gap produces an evidence-backed recommendation. |
| Insufficient evidence | The process records the need but does not invent an answer. |
| Existing answer | A rephrased question points to its existing FAQ entry. |
| Out of scope | The decision and reason are shown without drafting an answer. |
| Security | Personal or confidential information and malicious instructions or code are kept out of logs and candidate repositories. |
| Business opportunity | A recurring need suited to a project is recorded in the separate opportunity register. |
| Repeatability | Re-running the same versioned mock inputs preserves stable record identities and makes outcomes comparable; model wording may vary. |
| Human decision and publication boundary | The owner can accept, edit or reject a recommendation, while the active FAQ catalogue remains unchanged. |

## Architectural considerations

| Concern | Position |
| --- | --- |
| **Security** | Use mocked inputs and approved reference material. Screen and sanitize content before agent processing; sensitive or malicious content is excluded from logs and both candidate repositories. The demonstration has no active customer-data connection or knowledge-base write access. |
| **Reliability** | Validate inputs and recommendations, and fail closed when evidence or required data is missing. Record stable identifiers and the versions of mock inputs and process configuration so runs can be repeated and compared; do not promise identical model wording. |
| **Performance efficiency** | Group questions before agent processing and retrieve relevant passages rather than whole documents. The demonstration is a batch workflow, so completion within a live customer response window is not a requirement. |
| **Cost effectiveness** | Use deterministic grouping and existing-FAQ checks to reduce unnecessary model work. Measure the work across the full mock batch rather than reporting a selectively favorable example. |
| **Operational excellence** | Keep recommendations traceable to their evidence and decision rationale. Use repeatable evaluations to identify failures and improve the process over time. The demonstration corpus is synthetic and does not establish performance on live support data. |
| **Deployment** | No production deployment is included. The interface demonstrates the process locally using mocked JSON and approved demonstration material. |
| **Platform decisions** | Google ADK for the single agent; Python for implementation; Pydantic for validation; GitHub for source and version control; OpenSpec for requirements and change tracking. |

## Team

Three people share the work and the presentation deck.

| Role | Owner |
| --- | --- |
| Lead | Yasin Durgun |
| Repository owner | Fabian William Bakker |
| Evaluation owner | Siva Prasada Reddy Sogalapalli and Fabian William Bakker |
| Deck owner | Yasin Durgun, Fabian William Bakker and Siva Prasada Reddy Sogalapalli |

Everyone codes. Siva originated the idea.

## Sign-off

| | |
| --- | --- |
| **Decision** | Approved |
| **Approved by** | Capstone Committee |
| **Date** | 19 October 2026 |


