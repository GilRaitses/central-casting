# Central Casting: kill and respawn a coding agent without leaking context

Source document for a short event demo. It is written to be read on its own and to be fed into a slide generator. Each section maps to one or two slides, and the figure blocks describe visuals a generator can render.

## One-line pitch

How to kill and respawn a coding agent in Cursor without leaking context, and cut token spend by roughly 70 to 85 percent, using state on disk and an external orchestrator you can cycle.

## Who this is for and what they want

The audience builds with coding agents and wants two things. First, spend less. Second, kill and respawn an agent without losing or polluting its context. This demo answers both with a concrete method and a real cost figure.

## The problem

A single long-lived agent re-sends its whole growing context on every turn. Input tokens dominate the bill, so cost climbs worse than linearly as a session runs. Orchestration makes it worse, because planning, routing and reconciliation are token-heavy reasoning that never touches code, yet they burn premium in-editor turns. Two structural moves fix this: put the durable state on disk so the chat is disposable, and move the planning to a separate chat you can cycle.

Figure 1. A line that rises steeply: x axis is turns in a session, y axis is cumulative input tokens billed. One curve labeled "single long agent" bends upward. A second flat-ish curve labeled "bounded workers, respawned" stays low. Caption: the context tax.

## The .cca folder: state on disk

The durable truth lives in a folder called `.cca`, with one lane per work area. The chat is disposable and the folder is the source of truth.

```
.cca/
  O1 .. O16/                  a lane, the local orchestrator's home
    current_step.yaml         lane step state
    MMDD_task/                a dated task home
      A1 .. An/               a worker actor in the lane
        drafts/               work the worker produced
        current_state.yaml    the worker's externalized state
```

A worker's state is its `current_state.yaml` plus its `drafts/`. Nothing irreplaceable lives in the chat. That single property is what makes a clean kill and respawn possible.

Figure 2. A folder tree drawn as nested rounded boxes, matching the structure above, with the lane box and the worker box highlighted. Caption: state externalized to disk per lane and per worker.

## Hydration: the kill and respawn

When a worker slows or fills its context, you checkpoint it to disk, kill it, and spawn a fresh worker that hydrates from the saved state.

Steps:
1. Worker is running and slows or fills context.
2. Checkpoint to disk: write `current_state.yaml` and the `drafts/`.
3. Kill the worker. Context goes to zero.
4. Spawn a fresh worker with an empty context.
5. Hydrate: read `current_state.yaml` and only the drafts it needs.
6. Resume the same loop with a fresh agent.

The fresh worker reads a few KB of state, not the prior thread. There is no replay tax and no stale or cross-task context carried in, which is what "without leaking context" means in practice.

Figure 3. A horizontal flowchart: Worker running, then Checkpoint to disk, then Kill, then Fresh worker, then a return arrow down to Hydrate and resume. The Checkpoint and Kill boxes use a warm accent color. Caption: the hydration loop.

## The external orchestrator, and cycling it

Planning, routing and state reconciliation run in a separate chat called O0, outside the editor. Because O0 treats the `.cca` workspace as the source of truth and reconciles from it every turn, the chat holds nothing irreplaceable. When a chat slows or fills, you start a fresh one, reload the portable instruction layer and reconcile from the workspace.

Figure 4. A horizontal flowchart: O0 chat slows, then Fresh O0 chat loads the instruction layer, then Reconcile from .cca, then Resume routing. Caption: cycling the orchestrator costs one hydration turn.

## What the v5 O0 rules mean, in plain terms

The cycling works because the orchestrator follows four rules. They are written tersely in the system, so here they are for a human reader.

1. The workspace is the truth, not the chat. The `.cca` bundle is the primary state surface. O0 reads it every turn and treats it as authoritative over anything it remembers.
2. Reconcile before acting. Before O0 interprets any worker return or operator request, it compares that input against the latest workspace state. If they disagree, it stops and writes a reconciliation surface, then proceeds once they agree.
3. No stale memory. O0 refuses to rely on inferred continuity or prior-phase assumptions. This is the rule that makes the chat disposable, because nothing important is allowed to live only in the chat.
4. The instruction layer is portable. The rules load into any fresh chat as a single block, so a new O0 is the same O0 after one hydration turn.

Put together, a slow or full O0 chat is not a loss. You open a new one, paste the instruction layer, point it at the workspace, and it picks up exactly where the last one was.

## What it costs, on a real program

These numbers come from one real five-week run of this system, read from its own step log. The run covered April 23 to May 30, 2026.

Real anchors:
- 7 active lanes
- 14 task homes
- 175 recorded checkpoints
- about 12 external O0 chats cycled as they filled

Headline: roughly 70 to 85 percent fewer input tokens on the metered surface. The durable claim is the token reduction. Dollars are an illustration.

Two levers drive it.

Lever one, the external orchestrator. About 12 context-filling O0 chats ran on a flat-fee chat subscription, off the metered in-editor turns, and a slowing chat was cycled in one hydration turn. Estimated effect: on the order of 12 to 24 million input tokens moved off the metered surface, near 40 to 70 dollars at 3 dollars per million input tokens, replaced by about 20 dollars a month flat.

Lever two, bounded workers and respawn. Each worker ran scoped to one task home, near 10 to 25 thousand tokens of context, where one monolithic thread grows to 80 to 150 thousand. Estimated effect: about 70 to 80 percent fewer input tokens per task, and a slow worker costs one hydration turn to replace.

Assumptions and caveats: the numbers depend on the model, the pricing mode and prompt caching. Caching lowers the absolute dollars, and the structural win of bounded context plus respawn from disk still cuts both cached and uncached load.

Figure 5. A simple two-bar chart. Bar one, "single long agent," tall. Bar two, "Central Casting," short, about one fifth the height. Y axis is input tokens per task. Caption: input tokens per task, before and after.

## Demo walkthrough

1. Show the cost. One long agent re-bills a growing context every turn, and planning burns premium turns that never touch code.
2. Open the .cca folder. State is externalized to disk per lane and per worker, so the chat is disposable.
3. Kill and respawn. Checkpoint a worker, kill it, spawn a fresh one and watch it hydrate from a few KB of state.
4. Cycle the orchestrator. Replace a slow O0 chat with a fresh one that reconciles from the workspace in a single turn.
5. Show the number. The token reduction, anchored to the real five-week run.

## Suggested slide outline

1. Title. Kill and respawn a coding agent without leaking context.
2. Who this is for. Spend less, and never lose an agent's place.
3. The cost problem. Figure 1, the context tax.
4. The fix in one sentence. State on disk, orchestrator you can cycle.
5. The .cca folder. Figure 2, the folder tree.
6. Hydration. Figure 3, the kill and respawn loop.
7. The external orchestrator. Figure 4, cycling the orchestrator.
8. The v5 rules in plain terms. The four rules.
9. The cost, on a real program. The four anchors and the headline.
10. The number. Figure 5, before and after.
11. Walkthrough recap and where to try it.

## Glossary

- O0: the external orchestrator, the zero element of the lane system. It plans, routes and reconciles, and it does not execute.
- Lane: one work area, O1 through O16, each with a local orchestrator home.
- Task home: a dated folder inside a lane that holds one task.
- Worker actor: an agent that does the execution inside a task home, named A1 through An.
- Checkpoint: a recorded state change, the unit of memory.
- Hydration: rebuilding a fresh agent from the state on disk.
- Cycling: replacing a slow or full chat with a fresh one that reconciles from the workspace.

## Honest scope

The method is recent and still maturing. The discipline reduces context loss and token cost, and it depends on care. The cost figures are estimates from one real run, not a benchmark. The structural claims, state on disk, bounded context and a cyclable orchestrator, hold regardless of the exact dollar amount.

## Where to try it

- Event page: https://gilraitses.github.io/central-casting/event.html
- Method walkthrough: https://gilraitses.github.io/central-casting/walkthrough.html
- In practice on a real program: https://gilraitses.github.io/central-casting/aimez-program.html
- Source: https://github.com/GilRaitses/central-casting
