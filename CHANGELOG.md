# Changelog

Releases of Yasuo Coach. Each entry describes what changed for someone using the app, not
what changed in the code.

## v0.4.2 — 2026-09-06

Three things the coach was getting wrong in a real game, each found by playing one and then
measured against the recording.

- **It no longer says your opponent is at base while they are standing in front of you.**
  League's own data feed does not tell an app what an enemy bought at the moment they buy
  it — it reveals their items when your team can see them again. The coach was reading that
  reveal as a fresh purchase and calling "they just bought, they're at base, shove now" at
  the exact moment they walked back into lane. That call is gone for good. For the same
  reason the item warning no longer claims an opponent *just finished* something: it says
  what they have — "Malzahar has Blackfire Torch" — which is the part their loadout can
  actually prove, and the part the advice hangs on.
- **Objective calls now come 45 to 30 seconds before the spawn**, instead of a minute and a
  half. Every objective line in the measured game spoke at exactly 90 seconds — far enough
  out that the wave you were told to push had come and gone by the time it mattered. A call
  that cannot be made inside that window is no longer made at all.
- **The panels you read can be resized, and stay resized.** Drag the bottom edge of the
  coach output, the list of what was said this game, or any of the analysis, sessions,
  history and cleanup panes. The app reopens each one the size you left it. The coach output
  in particular used to shrink to a few lines whenever anything else was on the screen.

Two files: `YasuoCoach-setup.exe` (installer, SHA-256
`bf65167d391f3021bc404d1157cd9d4adb6596b80c08abeec38d6f2ce8ab91fa`)
and `YasuoCoach-portable.zip` (the same app as a folder, SHA-256
`a74746873ec00319ff8cd39e24dfe61e4329d05d5e773d083d09e04e4ecc763d`).

## v0.4.1 — 2026-09-06

A point release on v0.4.0, closing the last thing that still needed a command prompt.

- **Build a deeper match history from the app.** One call to Riot reaches at most 20 games,
  so a longer history is fetched in pages. The History screen now has a **Skip the newest**
  box: sync 20, set it to 20 and sync again, then 40. Before this, that was the one job the
  window could not do.

Two files: `YasuoCoach-setup.exe` (installer, SHA-256
`7b488a24ef9ad5e37f512bad3945fb60e4e1970e81ae712b0f600acb65c64dc5`)
and `YasuoCoach-portable.zip` (the same app as a folder, SHA-256
`d7f82a4d8d2b3356dc37c6beb2aef0cb6ad5ce1a4f9901f48639ae6c2989c722`).

## v0.4.0 — 2026-09-05

Two files: `YasuoCoach-setup.exe` (installer, SHA-256
`9e721764f1b0c14efb4f858b8b051996ebc1ad9327df4959efb151ec3959be3f`)
and `YasuoCoach-portable.zip` (the same app as a folder, SHA-256
`eac1ddaaec55ac88c276050a518dcb7e13288f8c52953071e2b6918b212ba46f`).
Built from a clean checkout and checked with the app's own preflight before publishing.

Everything you do between games is now a screen. The previous release could coach a game
and little else without a command prompt; this one does the whole loop in the window.

- **A preflight that runs itself.** The Play screen checks this machine before you start —
  build, platform, resolution, whether League's own data feed is answering, which voice it
  would use, and WebView2 — and shows each answer with what it costs you if it is missing.
  No game running is reported as expected, not as a fault.
- **Grade a game, and change your mind.** Every line the coach spoke, with its clock, and
  two questions per line: was it fair, was it useful. A wrong click is no longer permanent
  — reopening an answer records a correction and keeps the first one, so the record of
  what you thought stays intact.
- **Your own match history, in the app.** Your recent games from Riot's public match API,
  and the analyses over them: where and when you die, and which of your own numbers differ
  between the games you won and the ones you lost. Fetching is the only thing in the app
  that opens a network connection, and only when you press the button.
- **Nowhere to save your API key, on purpose.** Type it in and it lives in that window
  until you close it, exactly as an environment variable does in a terminal, and it is
  cleared once a sync succeeds. It is never written to disk, never put on a command line,
  and never logged.
- **Reclaim disk without deleting your games.** Recordings are mostly screenshots. The
  Sessions screen measures what they occupy and can drop the screenshots of older games
  while keeping everything the coach needs to grade and re-analyse them. It shows exactly
  which games it would take, needs a second confirmation, and never touches the newest
  three or anything you have marked to keep.

Fixed from v0.3.0: the shipped `YasuoCoach.exe` embedded the build machine's user name in
its compiled-in file paths. It no longer does, and the release build carries no symbol
table.

What it deliberately does not do is unchanged: see [docs/BOUNDARY.md](docs/BOUNDARY.md).

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
