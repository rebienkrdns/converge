# Investigate

## Purpose

Gather evidence that can support or invalidate candidate directions. Evidence is not the same as opinion or preference. This phase produces the factual substrate that makes Debate and Decide defensible rather than arbitrary.

## Entry Condition

The structured problem definition from Understand is complete. The root question and success criteria are agreed upon.

## Process

1. Identify the evidence types needed to answer the root question: code analysis, performance data, architecture diagrams, incident history, test coverage reports, security audits, domain expert input, benchmark results, or external research.
2. Gather evidence from primary sources where possible. Code, logs, and measurements are primary. Verbal descriptions of those things are secondary.
3. Evaluate each piece of evidence for quality: is it recent, reproducible, and from the actual system under question, or from an analogous system that may not generalize.
4. Identify what evidence cannot be gathered within the current cycle and record why.
5. Document evidence gaps explicitly. A decision made with known gaps is traceable. A decision made without acknowledging gaps is a hidden assumption.
6. Identify what would change if a current assumption were proven false.

## Outputs

- Evidence record: each item with source, date, quality assessment, and relevance to the root question.
- Evidence gaps: items that are needed but unavailable, with the reason for unavailability.
- Assumption list: claims being treated as true for which evidence is absent or incomplete.
- Sensitivity analysis: which assumptions, if wrong, would most significantly change the direction.

## Exit Condition

The council agrees on what evidence is available, what is missing, and which assumptions carry the highest decision weight. No council member should be surprised by the evidence record at this point.

## Failure Signals

- Evidence is cited without a source.
- The team treats absence of evidence as evidence of absence.
- Investigation is driven by confirming a preferred solution rather than answering the root question.
- Evidence from an unrelated system is applied without checking for material differences.
- The investigation phase never ends because the team keeps finding new things to check.

## Interaction with Subsequent Phases

The evidence record is the primary input to Debate. Challenge uses the assumption list and sensitivity analysis to stress-test the framing. Evidence gaps inform the confidence level in Decide and may activate additional gates in Validate.

