# Documentation Gate

## Purpose

Block delivery when the decision cannot be understood, operated, or maintained by someone who was not part of the decision process, relying only on the available documentation.

## Applies When

The decision introduces a new component that will be operated by a team other than the one that built it, changes the behavior of an existing interface that other teams depend on, establishes an architectural pattern that future decisions will follow, or makes a commitment whose rationale will not be obvious from the code alone.

## Approval Conditions

All of the following must be true:

- A completed decision record (Technical Decision Record or equivalent) exists and covers: the decision statement, the context that made it necessary, the alternatives considered and their rejection rationale, the consequences (positive and negative), the reversal conditions, and the active assumptions.
- Operational documentation exists for any new component: at minimum, how to start and stop it, how to read its health and status, what to do when the most common failure modes occur, and who owns it.
- The decision record is accessible to everyone who will be affected by the decision — it is not stored only in the head of the proposer or in a private document.
- Terminology in the documentation is consistent with the project's established glossary. New terms introduced by the decision are added to the glossary before delivery.
- An engineer who joins the team six months from now and reads the documentation could understand what was decided, why, and what has changed since, without needing to interview the original team.

## Blocking Conditions

The gate blocks if any of the following is true:

- No decision record exists. A decision that is not documented did not happen in a form that can be maintained.
- The decision record exists but omits the alternatives considered and their rejection rationale. Future engineers will recreate the same analysis and may reach a different conclusion without knowing it was already done.
- Operational behavior is described only in comments in the source code, making it inaccessible to operators who do not read the source.
- Reversal conditions are absent from the decision record. Without reversal conditions, the decision cannot be revisited in a structured way.
- A new API contract is introduced without documentation of the request format, response format, error conditions, and versioning policy.

## Evidence Required

- Completed Technical Decision Record using the Technical Decision Record Template.
- Operational notes for any new component: startup, shutdown, health check, common failure modes, ownership.
- Confirmation that the decision record is stored in the team's shared, persistent documentation system.
- Glossary updates for any new terms introduced by the decision.

