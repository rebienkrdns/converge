# Observe

## Purpose

Capture what is actually happening in the real system before any interpretation begins. The goal is fidelity to reality, not explanation of it. Premature framing contaminates every downstream phase.

## Entry Condition

A trigger has been identified: a bug report, a feature request, a performance signal, an architectural concern, a security incident, or a strategic direction change. No solution hypothesis is required to enter this phase.

## Process

1. Record the trigger in its original form without paraphrasing.
2. Identify the system area, component, or user flow where the signal originates.
3. Collect observable facts: error messages, metrics, user feedback, logs, incident records, support tickets, or audit results.
4. Note what is absent: missing instrumentation, gaps in knowledge, unverifiable claims.
5. Identify who is affected and in what way.
6. Record the timeline: when the signal appeared, how long it has been present, what changed before it appeared.

## Outputs

- Raw trigger statement in its original form.
- List of observable facts with source and timestamp.
- List of unknown or unverifiable items.
- Affected stakeholders and system components.
- Initial scope boundary: what is inside the observation and what is explicitly outside.

## Exit Condition

The council can describe what is happening without disagreeing on the facts. Interpretations may differ; facts should not.

## Failure Signals

- The team skips directly to solution proposals.
- Observations are already phrased as diagnoses ("the problem is X").
- Facts and assumptions appear in the same list without distinction.
- No one can agree on what the actual trigger was.
- Important context is missing because nobody thought to record it.

## Interaction with Subsequent Phases

The output of Observe is the only uncontaminated input the cycle receives. Understand, Investigate, and Challenge all depend on the fidelity of the observation record. A polluted observation produces a polluted decision.

