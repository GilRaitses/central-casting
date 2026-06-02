# central-casting

A public demo of a method for turning one open conversation thread about a
project into a structured, multi-session system: work lanes, system surfaces,
task home folders and schemas. The operating layer is Cursor.

## What is here

- `WALKTHROUGH.md` is the method, written as eight steps. It reads on its own and works as the source for a short demo deck.
- `example/` is a sanitized schema set the walkthrough points at. It carries no private project content.
  - `00_orchestrator.md` defines the zero element that routes work and reconciles state.
  - `actor_catalogue.yaml` lists the example work lanes.
  - `system_surfaces.md` defines the file surface classes.
  - `work_home_schema.yaml` defines the task home folder.
  - `checkpoint_policy.yaml` defines checkpoints as the unit of memory.
  - `STEP_LOG.example.md` shows a real checkpoint history.
- `index.html` is the live demo page, served through GitHub Pages.
- `aimez-program.html` is the worked counterpart: the demo catalogue, the tooling layers and how the aimez.ai research program runs on this method.

## How to use it

Read `WALKTHROUGH.md`, then open the matching file in `example/` at each step.
The live page presents the same eight steps in order.

## Honest scope

The method is recent and still maturing. It reduces context loss across long,
multi-session work, and it depends on care. This repository is a teaching demo,
not a framework to install.
