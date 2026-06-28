# Central Casting: the complete demo source

This is the single source to feed into NotebookLM. It consolidates every page of the Central Casting demo into one document: the method, the orchestrator, the demo catalogue, how the aimez.ai program runs on it, the cost demo about kill-and-respawn and long-run spend, and the v5 00 rules in plain terms. It is written to read on its own and to drive a slide deck. The figure blocks describe visuals a generator can render. The slide outline near the end gives a deck structure.

Live pages this source covers:
- Demo home: https://gilraitses.github.io/central-casting/
- Method walkthrough: https://gilraitses.github.io/central-casting/walkthrough.html
- Reference and worked example: https://gilraitses.github.io/central-casting/reference.html
- In practice on a real program: https://gilraitses.github.io/central-casting/aimez-program.html
- Cost demo, kill-and-respawn and long-run spend: https://gilraitses.github.io/central-casting/event.html
- Source: https://github.com/GilRaitses/central-casting

## One-line pitch

How to take one open conversation thread and grow it into a structured, multi-session agent system that holds its own memory, then how that same system lets you kill and respawn an agent in Cursor without leaking context and cut token spend by roughly 70 to 85 percent.

## Who this is for and what they want

The audience builds with coding agents. They want two things. First, spend less. Second, kill and respawn an agent without losing or polluting its context. This demo answers both with a concrete method, a real folder layout and a real cost figure.

## What Central Casting is

Central Casting is a method for keeping long, multi-repository agent work coherent and auditable. It assigns a cast of agent lanes, where each lane is a team with explicit inputs, outputs and an escalation rule, run by a local orchestrator. A zero element called 00 sits above the lanes and orchestrates. The operating layer is Cursor. The durable state lives on disk, so the chat is disposable.

## The problem it solves

A long-lived agent re-sends its whole growing context on every turn, so input tokens climb worse than linearly, and a cold session re-reads the thread and re-derives its decisions before it does any work. The gap is measurable, the context a run carries and re-bills, and across several repositories the work also drifts apart. The method closes the gap by giving the work a structure on disk that holds its own memory, so a session weeks later resumes from recorded state.

## The method, in nine steps

1. Start from the open thread. Name the project and its scope in one sentence, so everything after has a fixed center.
2. Name the work lanes. List the distinct kinds of work. Each becomes a lane, a team with a lead actor that spawns supporting actors as the work compounds. A zero element called 00 orchestrates above them.
3. Set up system surfaces. Give every file a surface class with one job: authoritative memos, derived reports, narrative logs or current state. The path alone then tells a reader what to trust.
4. Create task home folders. Each piece of work gets a dated task home named yyyymmdd_slug, carrying a manifest, a readme, a step log and handoff notes.
5. Add the schemas. The catalogue schema defines the lanes and folder rules. The work-home schema defines what every task folder must contain.
6. Record checkpoints as memory. The unit of memory is a checkpoint, a recorded state change in what a lane knows, can do, is blocked by or is allowed to write.
7. Keep a local and cloud boundary. The memory and audit layer stays local. Production work such as formatting and cross-repository assembly can be delegated to cloud agents.
8. Hand off in writing. When work moves between lanes or sessions, the handoff is written down in the task home, so the next agent has the context intact.
9. Dispatch a waveset. 00 charters a whole campaign and dispatches it to a fresh orchestrator, a background subagent or a separate thread, that runs the campaign as waves of parallel subagents and reports back while 00 stays free for other work. A subagent is an actor and each one owns one bounded task and one disjoint set of files. The dispatched orchestrator answers to 00, not to the operator. This layer is recent and still maturing. It extends the existing method rather than replacing it.

## System surfaces

A system surface is a class of file with one job. Naming the surface class of every file keeps a multi-session project legible. The example uses four classes:
- Memos, authoritative contracts and decisions, changed only through a new superseding memo.
- Reports, derived artifacts and analysis outputs, writable in execution.
- Logs, day logs and narrative, writable when a lane is authorized.
- State, current system state, read-only during proposal and intake.

Each file resolves to exactly one surface class, so a reader knows from the path alone whether a file is authoritative, derived, narrative or state.

## The orchestrator, 00

00 is the zero element of the lane system and the central orchestrator. It is a control surface, not an execution surface.

What 00 does: intake and classification, routing a bounded task to the right lane, state reconciliation before interpreting any return, and memo generation that writes the contract a lane executes against. It also dispatches a waveset, a whole campaign it charters and hands to a fresh orchestrator that runs waves of parallel subagents and reports back while 00 stays free for other work.

What 00 does not do: it does not execute lane work, it does not mutate repositories or documents, and it does not stand in for a lane's own validation.

Keeping orchestration separate from execution is what lets the lanes stay focused and the state stay trustworthy. 00 holds the map and the lanes do the work.

## The four columns

The program is organized along four columns the catalogue keeps separate, so any file or action resolves to a clear place.
- Instrumentation: the tool layers that produce work, from open-ended reasoning to in-editor drafting.
- Actors: the workers inside each lane, named by the lane letter and an instance number, run by the lane's local orchestrator.
- Contracts: the system surfaces and written handoffs that say what each lane reads and writes.
- ID: the stable index that names each lane by number and letter.

## Instrumentation, the drafting layers

Open-ended planning runs in a general chat so heavy orchestration reasoning stays off the in-editor token budget. Cursor reads and writes the repository. GitHub Copilot drafts inline. Every layer is grounded in repository source, so the work stays anchored to what is committed.

## The demo catalogue

00 is the central orchestrator. The catalogue indexes sixteen work lanes by letter, A through P. The ID is the lane's local orchestrator, 01 through 05, which records step state for its actors and relays verified summaries up to 00. The actors that do the work in a lane are named by the lane letter and an instance number. The letter O names the orchestration lane itself, so it carries no separate worker team.

| ID | Letter | Lane |
|---|---|---|
| 00 | zero element | Orchestration and coordinating. Intake, routing, state reconciliation, catalogue hygiene and cross-repo packaging. It does not execute lane work. |
| 01 | A | Authority / System / Excavation / Validation / Chain Execution |
| 01 | B | Research / Analysis / Experimental Lab |
| 02 | C | Control System Infra Unification |
| 03 | D | UX / Documentation |
| 04 | E | Patenting / Authoring Strategy |
| 05 | F | Packaging / Boundary Tracking / Presentation Slides |
| 03 | G | Theoretical Research / Interdisciplinary Collaboration |
| 04 | H | Topological Modeling / Hatcher |
| 05 | I | Integration / Frontend Testing |
| 01 | J | Packaging and presentation |
| 02 | K | Research and analysis |
| 03 | L | Modeling and experiments |
| 04 | M | Writing and documentation |
| 05 | N | Data pipelines and conversion |
| 00 | O | The orchestration lane itself. 00 is the central orchestrator and 01 through 05 are the per-lane local orchestrators, so the O letter carries no separate worker team. |
| 05 | P | Packaging and presentation. Presentation, funding and paper-drafting coordination. |

## How aimez.ai runs on it

The aimez.ai research program is the work this catalogue coordinates. The deployment runs along two strands: the site and routing work, and the research grounding and manuscript chain.

Site, product and routing:
- 00 orchestration: cross-repo orchestration, packaging the program across the site, the manuscripts and the public demos.
- 03 UX and documentation: aimez site identity, the notes pages and the figures gallery.
- 04 patenting and authoring strategy: the StreetLight validation brief and provider disclosure boundaries.
- 05 packaging: the advisor outreach packet and grant proposal revision alignment.
- 03 theoretical research: the research partner packet, formal theory sharpness and the allocentric flocking bridge.
- 04 topological modeling: the topological framing of the Manhattan graph work.
- 03 product alignment: aimez program identity, holding the program coherent across surfaces.

Research grounding and manuscript chain, where the catalogue has cast multiple actors per lane:
- 01 authority and excavation, actors A1 and A2: the trace reconstruction lane, excavation lineage and chain execution behind the published figures.
- 01 research and lab, actors B1 and B2: the evidence closure lane and the figure-data chain.
- 05 publication, actors 05-A1 and 05-A2: the exploratory findings manuscript chain, with 05-A1 the research counterpart and P1, P2 the publication runners.

Some lanes are live and some are cast and held. The migration registry in the program repository records the current step for each lane and actor.

## The cost problem

A single long-lived agent re-sends its whole growing context on every turn. Input tokens dominate the bill, so cost climbs worse than linearly as a session runs. Orchestration makes it worse, because planning and reconciliation are token-heavy reasoning that never touches code, yet they burn premium in-editor turns. Two structural moves fix this: put the durable state on disk so the chat is disposable, and move the planning to a separate chat you can cycle.

Figure 1. A rising line: x axis is turns in a session, y axis is cumulative input tokens billed. One curve labeled "single long agent" bends upward. A second flat-ish curve labeled "bounded workers, respawned" stays low. Caption: the context tax.

## The .cc folder, state on disk

The durable truth lives in a folder called .cc, one lane per work area. The chat is disposable and the folder is the source of truth.

```
.cc/
  01 .. 05/                  a lane, the local orchestrator's home
    current_step.yaml         lane step state
    MMDD_task/                a dated task home
      A1 .. An/               a worker actor in the lane
        drafts/               work the worker produced
        current_state.yaml    the worker's externalized state
```

A worker's state is its current_state.yaml plus its drafts. Nothing irreplaceable lives in the chat. That single property is what makes a clean kill and respawn possible.

Figure 2. A folder tree drawn as nested rounded boxes, matching the structure above, with the lane box and the worker box highlighted. Caption: state externalized to disk per lane and per worker.

## Hydration, the kill and respawn

When a worker slows or fills its context, you checkpoint it to disk, kill it and spawn a fresh worker that hydrates from the saved state.
1. Worker is running and slows or fills context.
2. Checkpoint to disk: write current_state.yaml and the drafts.
3. Kill the worker. Context goes to zero.
4. Spawn a fresh worker with an empty context.
5. Hydrate: read current_state.yaml and only the drafts it needs.
6. Resume the same loop with a fresh agent.

The fresh worker reads a few KB of state, not the prior thread. There is no replay tax and no stale or cross-task context carried in, which is what "without leaking context" means in practice.

Figure 3. A horizontal flowchart: Worker running, Checkpoint to disk, Kill, Fresh worker, then a return arrow down to Hydrate and resume. The Checkpoint and Kill boxes use a warm accent color. Caption: the hydration loop.

## The external orchestrator, and cycling it

Planning, routing and state reconciliation run in a separate chat called 00, outside the editor. Because 00 treats the .cc workspace as the source of truth and reconciles from it every turn, the chat holds nothing irreplaceable. When a chat slows or fills, you start a fresh one, reload the portable instruction layer and reconcile from the workspace.

Figure 4. A horizontal flowchart: 00 chat slows, Fresh 00 chat loads the instruction layer, Reconcile from .cc, Resume routing. Caption: cycling the orchestrator costs one hydration turn.

## The v5 00 rules, in plain terms

The cycling works because the orchestrator follows four rules. They are written tersely in the system, so here they are for a human reader.
1. The workspace is the truth, not the chat. The .cc bundle is the primary state surface. 00 reads it every turn and treats it as authoritative over anything it remembers.
2. Reconcile before acting. Before 00 interprets any worker return or operator request, it compares that input against the latest workspace state. If they disagree, it stops and writes a reconciliation surface, then proceeds once they agree.
3. No stale memory. 00 refuses to rely on inferred continuity or prior-phase assumptions. This is the rule that makes the chat disposable, because nothing important is allowed to live only in the chat.
4. The instruction layer is portable. The rules load into any fresh chat as a single block, so a new 00 is the same 00 after one hydration turn.

Put together, a slow or full 00 chat is not a loss. You open a new one, paste the instruction layer, point it at the workspace and it picks up exactly where the last one was.

## What it costs, on a real program

These numbers come from one real run of this system, read from its own step log, covering April 23 to May 30, 2026.

Real anchors:
- 7 active lanes
- 14 task homes
- 175 recorded checkpoints
- about 12 external 00 chats cycled as they filled

Headline: roughly 70 to 85 percent fewer input tokens on the metered surface. The durable claim is the token reduction. Dollars are an illustration.

Lever one, the external orchestrator. About 12 context-filling 00 chats ran on a flat-fee chat subscription, off the metered in-editor turns, and a slowing chat was cycled in one hydration turn. Estimated effect: on the order of 12 to 24 million input tokens moved off the metered surface, near 40 to 70 dollars at 3 dollars per million input tokens, replaced by about 20 dollars a month flat.

Lever two, bounded workers and respawn. Each worker ran scoped to one task home, near 10 to 25 thousand tokens of context, where one monolithic thread grows to 80 to 150 thousand. Estimated effect: about 70 to 80 percent fewer input tokens per task, and a slow worker costs one hydration turn to replace.

Why a real bill reaches the low thousands. The savings are multiplicative. Premium model pricing, large re-sent context, many turns, many sessions over months and avoided re-runs all stack. A heavy metered cycle on usage-based pricing runs to dozens of invoices near 100 to 200 dollars each, so a single cycle is already low thousands, and the structural cuts above apply across every cycle.

Assumptions and caveats: the numbers depend on the model, the pricing mode and prompt caching. Caching lowers the absolute dollars, and the structural win of bounded context plus respawn from disk still cuts both cached and uncached load.

Figure 5. A two-bar chart. Bar one, "single long agent," tall. Bar two, "Central Casting," short, about one fifth the height. Y axis is input tokens per task. Caption: input tokens per task, before and after.

## Demo walkthrough

1. Show the cost. One long agent re-bills a growing context every turn, and planning burns premium turns that never touch code.
2. Open the .cc folder. State is externalized to disk per lane and per worker, so the chat is disposable.
3. Kill and respawn. Checkpoint a worker, kill it, spawn a fresh one and watch it hydrate from a few KB of state.
4. Cycle the orchestrator. Replace a slow 00 chat with a fresh one that reconciles from the workspace in a single turn.
5. Show the number. The token reduction, anchored to the real run above.

## Suggested slide outline

1. Title. Kill and respawn a coding agent without leaking context.
2. Who this is for. Spend less, and never lose an agent's place.
3. What Central Casting is. A cast of lanes, a zero-element orchestrator, state on disk.
4. The problem. Long threads lose state and re-bill context.
5. The cost problem. Figure 1, the context tax.
6. The fix in one sentence. State on disk, an orchestrator you can cycle.
7. The .cc folder. Figure 2, the folder tree.
8. Hydration. Figure 3, the kill and respawn loop.
9. The external orchestrator. Figure 4, cycling the orchestrator.
10. The v5 rules in plain terms. The four rules.
11. The demo catalogue. The lane table and the four columns.
12. How aimez.ai runs on it. The lane mapping.
13. The cost, on a real program. The four anchors and the headline.
14. The number. Figure 5, before and after. Close with where to try it.

## Glossary

- 00: the central orchestrator, the zero element of the lane system. It plans, routes and reconciles, and it does not execute.
- Local orchestrator: a per-lane orchestrator, 01 through 05, that records step state for its lane and actors and relays verified summaries up to 00.
- Lane: one work area, a team with a lead actor and supporting actors under its local orchestrator.
- Task home: a dated folder inside a lane that holds one task.
- Worker actor: an agent that does the execution inside a task home, named by the lane letter and an instance number, so lane B runs B1 and B2.
- System surface: a file class with one job, namely memos, reports, logs or state.
- Checkpoint: a recorded state change, the unit of memory.
- Hydration: rebuilding a fresh agent from the state on disk.
- Cycling: replacing a slow or full chat with a fresh one that reconciles from the workspace.
- Waveset: a whole campaign that 00 charters and dispatches to a fresh orchestrator to run as waves of parallel subagents, while 00 stays free.
- Dispatched orchestrator: a background subagent or a separate thread that receives a waveset, runs its waves and reports results back to 00. It answers to the orchestrator that sent it.
- Wave: one round of parallel subagents inside a waveset, where a subagent is an actor and each one owns one bounded task and one disjoint set of files. A wave repeats when a check fails.

## Honest scope

The method is recent and still maturing. The discipline reduces context loss and token cost, and it depends on care. The cost figures are estimates from one real run, not a benchmark. The structural claims, state on disk, bounded context and a cyclable orchestrator, hold regardless of the exact dollar amount.

## Where to try it

- Event page: https://gilraitses.github.io/central-casting/event.html
- Method walkthrough: https://gilraitses.github.io/central-casting/walkthrough.html
- In practice on a real program: https://gilraitses.github.io/central-casting/aimez-program.html
- Source: https://github.com/GilRaitses/central-casting
