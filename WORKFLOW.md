# WORKFLOW.md — Superwarp

## Purpose
Extend Final Fantasy XI's teleport systems without interrupting normal gameplay, preserving predictable and recoverable behavior across multiple Windower 4 clients.

This working guide adopts the applicable engineering, verification, and package conventions from NPCMirror's canonical `WORKFLOW.md`, consulted directly on 2026-11-05. The Superwarp-specific considerations below take precedence where necessary.

## Development principles
- Use legible, minimal, coherent changes. Read the live branch, upstream behavior, map files, and existing docs before patching.
- Keep the default `master` branch as the last version accepted after real in-game testing. Experimental changes belong on test branches until proven.
- Maintain compatibility with existing `//sw` contextual smart commands, destination abbreviations, per-character configuration, and direct `//hp`, `//wp`, `//sg`, etc.
- Make scope decisions exactly once at the issuing client. IPC receivers must execute locally, without re-dispatching.
- Never assume all multibox clients share an in-game party, a destination unlock, or a similar position. Report partial success per character.
- Do not alter packet handling, unlocked-destination checks, client-lock recovery, or send-all scheduling casually. Prefer observing actual completion over arbitrary sleeps.
- Scope changes to transport should not unexpectedly broadcast queries, local settings changes, or `hp set` return-point changes.
- Preserve one-letter destination commands (notably Sortie's `s` and `a`) when adding scope shorthand.
- Store user preferences via Windower's config settings under `data`; merge new defaults without discarding existing per-character settings.
- Favor small FFXI-style UI that can be hidden, dragged, recovered from off-screen positions, and configured without unnecessary chat messages or per-frame work.
- Treat assets from the Trust project as private local test inputs only. Do not copy its licensed source/images into this public repository or release without prior permission.

## Verification
1. Identify exact repository, branch, and last-known-good baseline.
2. Read `AGENTS.md`, `WORKFLOW.md`, and `TESTING.md` on the current branch and compare with baseline; if missing, consult NPCMirror's canonical workflow.
3. Make coherent commits, keeping the existing warp state machine unchanged unless the request specifically concerns it.
4. Check Lua syntax with a Lua 5.1-aware checker when available, then test scope parsing and fallback cases with static or simulated tests.
5. Test addon load/reload, direct and contextual warps, local party sizes 1/2/4/6, out-of-party clients, explicit solo/all overrides, failed/unlocked destinations, Sortie destinations, and received IPC messages inside Windower.
6. Record static/simulated versus real in-game evidence separately. Do not mark a feature as live-tested based on source review.
7. Re-read the original request, inspect the diff for unrelated changes, update `README.md` and cumulative `TESTING.md`, and report exact commit/push status, residual risks, and next checks.

## Documentation and delivery
- `README.md`: user controls, settings, examples, cumulative changelog at end.
- `TESTING.md`: current tests first, cumulative results/history and delivery register afterward; correct historical mistakes explicitly rather than deleting them.
- `AGENTS.md`: instructions for repository-aware development.
- Testing ZIPs: keep `README.md`, `WORKFLOW.md`, and `TESTING.md` inside the installable `superwarp` addon folder. Do not add duplicate copies at archive root.
- Each testing package corresponds to an identifiable Git commit on its development branch.
