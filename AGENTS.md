# AGENTS.md — Superwarp

Before changing Superwarp or preparing a test package, read the current root-level `WORKFLOW.md` and `TESTING.md` from the branch being edited. Do not rely on remembered copies.

Keep `master` as the last live-tested baseline. Use a development/test branch for work not yet tested inside FFXI.

Preserve the established warp packet state machines, menu handling, retry timing, zone mappings, and multibox staggering unless a change explicitly requires them. Treat IPC recipients separately from locally issued commands; never rebroadcast received commands.

Document changes, tests, known caveats, and commit identifiers. Distinguish static and simulated tests from actual Windower/FFXI observations. Keep the cumulative testing history; report precisely which files and branches changed and which actions remain unverified.

For testing packages, include `README.md`, `WORKFLOW.md`, and `TESTING.md` inside the installable `superwarp` folder. Do not include Trust's code or licensed images in a published Superwarp repository or release without permission.
