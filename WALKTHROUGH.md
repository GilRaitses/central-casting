# Central Casting: from a conversation thread to a structured project

This walkthrough shows how to take one open conversation thread about a project
and grow it into a structured, multi-session system: work lanes, system
surfaces, task home folders, and schemas. It uses Cursor as the operating
layer. The example files in `example/` are a sanitized demo, so nothing here
depends on any private project.

This document is written to be read on its own and to be fed into a slide
generator for a short demo deck. Each step is one idea.

## The problem

Long projects live in long threads, and each new session re-reads the thread,
re-derives decisions, and loses state, while work across several repositories
drifts apart. This method gives the work a structure that holds its own memory.

## Step 1: Start from the open thread

A project usually begins as one long conversation thread. That thread holds
intent, decisions, and dead ends, and it grows hard to reuse. The first move is
to name the project and its scope in a single sentence, so everything after has
a fixed center.

## Step 2: Name the work lanes

Read the thread and list the distinct kinds of work it contains. Each kind
becomes a lane, a team with a lead actor that spawns supporting actors as the
work compounds. The example catalogue uses research, data pipelines, modeling,
writing and packaging. A zero element called 00 sits above the lanes as the
central orchestrator. See `example/actor_catalogue.yaml` and
`example/00_orchestrator.md`.

## Step 3: Set up system surfaces

Give every file a surface class with one job: authoritative memos, derived
reports, narrative logs, and current state. A reader then knows from the path
alone whether a file is a contract, an output, a narrative, or live state. That
single property is what lets a new session trust what it reads. See
`example/system_surfaces.md`.

## Step 4: Create task home folders

Each piece of work gets a task home: one folder inside its lane, named
`yyyymmdd_slug`, carrying a fixed set of files (a manifest, a readme, a step
log, and handoff notes). A session opens the task home and hydrates from it. See
`example/work_home_schema.yaml`.

## Step 5: Add the schemas

Two schemas hold the system together. The catalogue schema defines the lanes
and the folder rules. The work-home schema defines what every task folder must
contain. Both are plain files a person and an agent can read.

## Step 6: Record checkpoints as memory

The unit of memory is a checkpoint: a recorded state change in what a lane
knows, can do, is blocked by, or is allowed to write. A session reads the recent
checkpoints and resumes from there. The checkpoint types cover hydration,
pre-write inventory, worker reports, gate transitions, blockers, handoffs,
commits, and authority changes. See `example/checkpoint_policy.yaml` and
`example/STEP_LOG.example.md`.

## Step 7: Keep a local and cloud boundary

The memory and audit layer stays local. Production work such as formatting,
packaging, and cross-repository assembly can be delegated to cloud agents.
Keeping that boundary explicit is a design choice that lets long-running work
stay coherent while the heavy steps run elsewhere.

## Step 8: Hand off in writing

When work moves between lanes or sessions, the handoff is written down in the
task home. Because the contract is explicit, one agent takes over from another
with the context intact.

## What this buys you

A session that starts weeks later opens the relevant task home, reads the recent
checkpoints, and continues with the thread intact. The conversation thread
becomes a system that holds its own memory.

## Glossary

- 00: the zero element and central orchestrator. It routes work and reconciles state, and leaves execution to the lanes.
- Local orchestrator: a per-lane orchestrator, numbered 01 through 016, that records step state for its lane and its actors and relays verified summaries up to 00.
- Lane: a team that owns one kind of work such as research, data pipelines, modeling, writing or packaging. It runs a lead actor and spawns supporting actors as the work compounds.
- Actor: a worker inside a lane, named by the lane letter and an instance number, so lane one runs A1 and A2 and lane two runs B1 and B2.
- System surface: a file class with one job, namely authoritative memos, derived reports, narrative logs or current state.
- Work home: one task folder inside a lane, named yyyymmdd_slug, with a manifest, a readme, a step log and handoff notes.
- Checkpoint: the unit of memory, a recorded state change in what a lane knows, can do, is blocked by or is allowed to write.
- Handoff: a written transfer of ownership or next-action authority between lanes or sessions.

## Honest scope

This method is recent and still maturing. The checkpoint discipline reduces
context loss; it depends on care, and a change in one lane reaches another only
when it is carried there on purpose. Naming the limit is part of the practice.
