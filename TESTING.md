# TESTING.md — Superwarp

## Current development batch — 2026-11-05
**Feature:** Party-by-default travel through universal `//sw` smart routing; explicit `s/p/a` scope overrides and saved `//sw scope` setting.
**Branch:** `test/party-default-scope-20261105` based on the unchanged `master` baseline.
**Status:** Source inspection and limited static review only. Not yet installed, loaded, or verified in live FFXI. No gameplay behavior has been observed or confirmed for this patch.

### Installation and intended test
Install a copy of this branch as `Windower/addons/superwarp` on a test setup, retaining a backup of the currently working folder (especially `data/settings_<Character>.xml`). Reload Superwarp and check:
1. `//sw scope` should report party. `//sw scope solo` and `//sw scope party` should persist after reload.
2. With one in-game party member, `//sw norg` near a Home Point should initiate only the issuing character's warp.
3. With two or more local Windower clients in the same FFXI party, `//sw norg` near an unlocked Home Point should stagger and confirm warps for party members only.
4. With a separate Windower client outside the party, `//sw norg` should not include that client. `//sw a norg` should include all local clients.
5. `//sw s norg` should warp the issuing character only. Also test `//sw p norg` and the scope-before-system fallback `//sw s hp norg`.
6. `//sw`, `//sw s`, and `//sw a` beside a Home Point and another compatible teleport system should preserve contextual smart behavior.
7. `//sw hp set` and `//sw hp missing` should remain local by default; explicit `//sw hp a set` should be tested only when intentionally changing each character's return Home Point.
8. Confirm legacy `//sw hp all norg`, `//sw hp party norg`, `//sw hp norg`, and direct `//hp` behavior.
9. Test Sortie single-letter destinations `//sw so a` and `//sw so s`, and explicit solo `//sw s so a`.
10. Verify failure, missing destination unlock, interrupted warp, reset, and partial-completion feedback. Confirm IPC receivers do not repeat broadcasts.

### Static inspection findings
- Existing `the_superwarp` and `magic_map` code choose the nearby warp system; no redundant new warp router was introduced.
- New default scope, explicit prefixes, saved setting, and recipient guard were added in `superwarp.lua`; the existing `sendall.lua` packet/IPC scheduling code and warp maps were not edited.
- A one-letter scope immediately after `//sw` intentionally takes precedence over an ambiguous destination; system-specific Sortie letters remain usable as `//sw so a` / `//sw so s`.
- A one-character party (or party with no other local clients) should skip IPC display based on source review.
- Lua 5.1 parser and actual Windower smoke tests have not yet been performed in this environment. Source-level inspection is not evidence of correct in-game behavior.

### Open issues and risks
- `//sw` chooses the nearest recognized supported NPC/device, not an arbitrary travel service; a requested destination may be unreachable through that system.
- `//sw s` and `//sw a` deliberately reserve these letters for scope; users needing a lettered Sortie destination must spell `//sw so s` / `//sw so a`.
- A single-letter scope placed after a warp-system name with no following argument remains a legacy destination token (collision avoidance), so prefer scope immediately after `//sw`.
- Per-character `//sw scope` settings may differ. IPC recipients execute received actions locally and must not fan out further.
- UI/Trust asset experiment, favorite Home Point commands, and fuzzy natural-language aliases are not part of this batch.

### Development record
- Scope-routing commit: `f95debfbc52704ba695670e1a6fde8fa71e7e518`.
- Sortie collision guard: `40c118ec8748e89bd52c94e7cf0969e3ea741026`.
- Updated README and adopted Superwarp-specific AGENTS/WORKFLOW practices on the same test branch.
- No test ZIP prepared or in-game tests recorded yet.

## Historical record
This is the first project-local cumulative testing record introduced on the development branch. Earlier Superwarp behavior and changelog are preserved in `README.md`; no prior `TESTING.md` existed in this repository at the time of adoption. Future testing batches must append results and retain this record.
