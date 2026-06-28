# 00, the orchestrator (example)

00 is the zero element of the lane system and the central orchestrator. It is a
control surface, not an execution surface.

## What 00 does
- Intake: take in a request or an open thread and classify it.
- Routing: send a bounded task to the right lane.
- State reconciliation: check current state before interpreting any return.
- Memo generation: write the contract that a lane executes against.
- Dispatch: charter a waveset and hand it to a fresh orchestrator that runs waves of parallel subagents and reports back, while 00 stays free for other work.

## Dispatching a waveset
A waveset is a whole campaign rather than one task. 00 writes the charter, then
dispatches it to a fresh orchestrator, a background subagent or a separate
thread. That dispatched orchestrator runs the campaign as waves of parallel
subagents, where a subagent is an actor and each one owns one bounded task and
one disjoint set of files. It reports each wave back to 00. The dispatched
orchestrator answers to 00, not to the operator, so escalation stays inside the
method. This layer is recent and still maturing. It extends the orchestrator
rather than replacing it.

## What 00 does not do
- It does not execute lane work.
- It does not mutate repositories or documents directly.
- It does not stand in for a lane's own validation.

## Why a zero element
Keeping orchestration separate from execution is what lets the lanes stay
focused and the state stay trustworthy. 00 holds the map; the lanes do the work.
