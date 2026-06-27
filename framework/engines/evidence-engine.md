# Evidence Engine

## Purpose

Collect, classify, and normalize every piece of evidence used in a decision so that the quality of the conclusion can be assessed independently of the conclusion itself.

## Inputs

- Code and architecture artifacts (source files, diagrams, dependency graphs).
- Operational data (metrics, logs, traces, incident records).
- Benchmarks and experiments (load test results, profiling output, A/B results).
- Domain expert input (named, with stated expertise and potential bias).
- Prior incidents or historical patterns (with dates and system context).
- External research (papers, documentation, case studies — with source and publication date).

## Outputs

- Evidence inventory: each item catalogued with source, date, type, and relevance.
- Quality classification per item: Primary / Secondary / Tertiary.
- Gap map: evidence that is needed to answer the root question but is not available.
- Contradiction log: items in the inventory that conflict with each other.

## Conceptual Algorithm

1. Assign each evidence item a source type:
   - **Primary**: directly observable from the system under review (logs, metrics, code, benchmarks on the actual system).
   - **Secondary**: derived from the system under review but through interpretation (expert analysis of code, EXPLAIN output, architectural inference).
   - **Tertiary**: from an analogous system or external source that may not generalize (case studies, documentation, expert opinion on a similar system).

2. Assess each item for recency: evidence older than the last significant system change may no longer be valid.

3. Identify contradictions: two items that cannot both be true. Record each contradiction without resolving it — resolution belongs to the Convergence Engine.

4. Map the gap: for each dimension the root question requires evidence on, confirm whether Primary or Secondary evidence exists. Gaps filled only by Tertiary evidence carry higher uncertainty and must be flagged to the Confidence Engine.

5. Reject assertions that have no traceable source. An assertion without provenance is an assumption, and it must be classified as such.

## Responsibilities

- Preserve provenance: every item in the inventory must be traceable to its source.
- Separate facts from inferences from opinions: each is valid evidence of a different type, but they are not interchangeable.
- Expose the gap map without minimizing it: a gap that is uncomfortable to acknowledge is more dangerous than one that is named.

## Interaction

Feeds the Confidence Engine (evidence quality determines confidence ceiling), the Impact Engine (evidence quality determines impact estimate reliability), and the Decision Matrix Engine (evidence rating is a matrix dimension). Contradiction log feeds the Convergence Engine.

