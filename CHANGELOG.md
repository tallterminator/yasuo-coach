# Changelog

Releases of Yasuo Coach. Each entry describes what changed for someone using the app, not
what changed in the code.

## v0.3.0 — 2026-09-05

The first public release. Two files: `YasuoCoach-setup.exe` (installer, SHA-256
`9346a96b5efa5f680268560590d1068689c7c76eeedc202793d472362d0eb80a`) and `YasuoCoach-portable.zip` (the same app as a folder, SHA-256
`8a73da0dbdb2ad94b7dcbaf6787fa54502ee336912e72dfc07e0330efe0d8139`). Built from a clean checkout on 2026-09-05 and checked with `coach doctor`
before publishing.

What the coach does in this build, for someone using it:

- **Knows who you are laning against** — decided from the kill feed, not the champion
  select label, so a lane swap does not fool it — and knows 31 champions: the lane rule
  at the first wave, what changes when they hit six, what twelve of their items mean for
  you, and why the first death to them happened.
- **Says each thing once.** Every spoken line is recorded with the exact words used, the
  same sentence is not repeated within four minutes, and lines expire instead of speaking
  late: item lines within 45 seconds, plate advice never after plates are gone.
- **Opens and closes windows.** "You have ult and they don't: force a fight" is retracted
  out loud the moment it stops being true.
- **Sees the map change.** Mid towers falling, the drake soul point, the enemy jungler
  showing on the map or going down, the enemy laner walking to base.
- **Speaks with a neural voice**, bundled; no download.
- **A Play screen for a player**: what the coach can see, every line it said, and why
  Start is disabled when it is.

What it deliberately does not do is unchanged: see [docs/BOUNDARY.md](docs/BOUNDARY.md).
