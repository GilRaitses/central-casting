# Example system surfaces (sanitized demo)

A system surface is a class of file with one job. Naming the surface class of
every file is what keeps a multi-session project legible. This example uses
four classes.

| Surface | Path | Role | Write rule |
|---|---|---|---|
| Memos | `.cc/memos/` | Authoritative contracts and decisions | Authoritative; change through a new superseding memo |
| Reports | `.cc/reports/` | Derived artifacts and analysis outputs | Writable in execution |
| Logs | `.cc/logs/` | Day logs and lab narrative | Writable when a lane is authorized |
| State | `.cc/state/` | Current system state | Read-only during proposal and intake |

## The rule that makes it work

Each file resolves to exactly one surface class. A reader knows, from the path
alone, whether a file is authoritative, derived, narrative or state. That
single property is what lets a new session trust what it reads.
