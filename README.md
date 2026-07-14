# central-casting

A public demo of a method for turning one open conversation thread about a
project into a structured, multi-session system: work lanes, system surfaces,
task home folders and schemas. The operating layer is Cursor.

## What is here

- `WALKTHROUGH.md` is the method, written as nine steps. It reads on its own and works as the source for a short demo deck.
- `example/` is a sanitized schema set the walkthrough points at. It carries no private project content.
  - `00_orchestrator.md` defines the zero element that routes work, reconciles state and dispatches wavesets.
  - `actor_catalogue.yaml` lists the example work lanes.
  - `system_surfaces.md` defines the file surface classes.
  - `work_home_schema.yaml` defines the task home folder.
  - `checkpoint_policy.yaml` defines checkpoints as the unit of memory.
  - `STEP_LOG.example.md` shows a real checkpoint history.
- `index.html` is the live demo page, served through GitHub Pages.
- `aimez-program.html` is a fictional worked example: the demo catalogue, the tooling layers and how a research program can run on this method.
- The primary walkthrough is a slide deck, `walkthrough-deck.html`. Every document also has a styled, native HTML page. Read them rendered at https://gilraitses.github.io/central-casting/ (`walkthrough-deck.html`, `walkthrough.html`, `reference.html`, `readme.html` and the pages under `example/`).

## How to use it

Read `WALKTHROUGH.md`, then open the matching file in `example/` at each step.
The live page presents the same nine steps in order.

## Honest scope

The method is recent and still maturing. It reduces context loss across long,
multi-session work, and it depends on care. This repository is a teaching demo,
not a framework to install. The waveset layer that dispatches parallel waves of
subagents is the newest part and the least settled. It extends the existing
method rather than replacing it.
