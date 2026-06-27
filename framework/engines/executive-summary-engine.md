# Executive Summary Engine

## Purpose

Compress the complete decision evaluation into a self-contained summary that communicates the decision, its rationale, the primary tradeoff, the gate status, and the next step to any stakeholder — including those who have not read the full decision record.

## Inputs

- Recommendation and rationale from the Recommendation Engine.
- Quality Score, Confidence, Evidence, and Impact from the Decision Matrix Engine.
- Gate status summary from the Gate Engine.
- Top two or three risks from the Impact Engine and the Self Critique system.
- Active assumptions with the highest decision weight.
- Next verification point from the Deliver phase.

## Outputs

- **Executive summary paragraph**: one paragraph, readable in under 60 seconds, containing the decision, the primary reason, the most significant tradeoff, and the recommendation.
- **Decision status**: a single-line status combining recommendation type and gate status (e.g., "Conditionally approved — pending Security Gate resolution").
- **Key tradeoffs**: up to three bullet points, each naming what is gained and what is given up.
- **Top risks accepted**: up to three bullet points, each naming the risk and the mitigation.
- **Active assumptions**: the highest-weight assumptions the decision depends on.
- **Next action**: the specific action required immediately after authorization, with a named owner.
- **Next verification point**: the date or observable condition at which the outcome will be evaluated.

## Conceptual Algorithm

1. Derive the decision statement from the TDR: one sentence identifying what was decided, in which system, and to what effect.

2. Derive the primary rationale: the single strongest reason the preferred option was chosen over the alternatives. This must be derivable from the Convergence Engine's preferred direction output, not invented.

3. Derive the most significant tradeoff: from the Convergence Engine's dominant tradeoffs output, select the one with the highest impact rating. State it as "X is gained at the cost of Y."

4. Select the top risks: from the Impact Engine's negative consequence summary, select the risks that are most material and accepted — not the most dramatic ones.

5. Derive the decision status: combine the Recommendation Engine's type with the Gate Engine's aggregate status. If any gate has a conditional pass, the status is conditional until all conditions are resolved.

6. Set the next action: the first concrete step after authorization. It must be a specific action (not "continue implementation"), with a named owner and a date.

## Responsibilities

- Preserve fidelity: the summary must not misrepresent the recommendation, the confidence level, or the tradeoffs to make the decision appear stronger or weaker than it is.
- Be self-contained: a reader who reads only the Executive Summary must understand what was decided, why, and what happens next. They must not need to consult the full record to act.
- Avoid oversimplification: if a conditional approval has three conditions, they must appear in the summary. Removing them to make the summary cleaner misrepresents the decision status.

## Interaction

Consumes all major engine outputs. Produces the artifact that is shared with leadership, adjacent teams, and future maintainers. The Executive Summary is the most frequently read output of the Converge cycle and the one most likely to be referenced after the context that produced it is gone.

