# Yasuo Coach

A speaking coach for League of Legends. It watches your game through the same things you
can see, and talks to you while you play — out loud, through a neural voice, plus an
overlay and a dashboard.

Windows only. Yasuo, mid lane, only.

---

## Install

Download **`YasuoCoach-setup.exe`** from the newest entry on the
[Releases](../../releases) page and run it. No admin rights (it installs under your user
profile), no Python, no other downloads: the voice is bundled. Windows may show a
SmartScreen prompt because the installer is unsigned; choose **More info**, then **Run
anyway**.

Prefer a folder? **`YasuoCoach-portable.zip`** is the same app unzipped; run
`YasuoCoach.exe` from inside it, and keep the folder somewhere you can write to (recorded
games are written next to it).

Then follow **[docs/QUICKSTART.md](docs/QUICKSTART.md)**: the preflight (`coach doctor`),
a two-minute smoke test, and your first coached game.

Current release: **v0.4.0** (2026-09-05). SHA-256 of the installer:
`9e721764f1b0c14efb4f858b8b051996ebc1ad9327df4959efb151ec3959be3f`.

---

## What it does

During a game it reads Riot's official Live Client Data API and your own screen, works out
what is happening, and speaks when it has something worth saying:

- **Lane state** — who you are actually up against (decided from the kill feed, not from
  the champion select label, because champions go anywhere now), level races, trading
  windows, when the enemy laner has walked to base
- **Map and objectives** — dragon and herald timers anchored to the clock, when to shove
  and move, when a fight is not yours to take
- **Deaths** — what happened, once, after the fight rather than during it

It tries hard to say *less*. A recent pass cut it from 381 lines a game to 300 by removing
everything that only restated what was already on your HUD.

**Between games** it can read your finished matches from Riot's public match API and find
patterns across them. That half is optional, needs your own API key, and never touches the
live coach.

## What it will not do

This is a passive coach. It reads only what you can already see and never acts for you:

- No game-memory reading
- No packet sniffing or network interception
- No client injection, DLL injection, or process hooking
- No input automation — it never sends a mouse or keyboard event to the game
- No hidden-state inference — no fog-of-war positions, no timers you cannot see, no enemy
  cooldown tracking

These are absolute. See **[docs/BOUNDARY.md](docs/BOUNDARY.md)** for the full statement,
including exactly which data sources are allowed and why.

## What it needs

| | |
| --- | --- |
| **Windows** | Windows 11 developed and tested; Windows 10 should work but is unverified |
| **League of Legends** | Installed. The coach reads `127.0.0.1:2999`, Riot's local endpoint, which is on by default |
| **Disk** | ~250 MB for the app itself (~120 MB plus a ~63 MB voice). **Recorded games are much bigger** — about 140 MB per minute, so roughly 4 GB for a 30-minute game. Recording is on by default; see [PRIVACY.md](docs/PRIVACY.md) |
| **Speakers or headphones** | It refuses to start a game it cannot be heard in, unless you pass `--no-speak` |

No Python, no admin rights, no account, and no API key for the live coach. The voice ships
inside the installer.

It runs at any resolution. Screen reading is fully measured at 1920×1080, partly measured
at 2560×1440, and scaled elsewhere — the app tells you which, and labels unverified reads
as unverified rather than pretending otherwise.

## Privacy

Everything stays on your machine. No telemetry, no account, no analytics, and no network
access at all while coaching. Recorded games are ordinary files in a folder you can
delete. See **[docs/PRIVACY.md](docs/PRIVACY.md)**.

## Why the source is not here

This repository carries the app, the license and the documentation — not the code. That is
a deliberate choice by the author, who is not open-sourcing the coaching logic.

Two things worth being straight about, rather than letting you discover them:

1. **The binary is inspectable.** It is packed with PyInstaller. Anyone determined enough
   can unpack and decompile it. Keeping the source private raises the effort and draws a
   clear legal line; it does not make the internals secret from someone who cares.
2. **You are trusting a binary from a stranger.** That is a real thing to weigh. The
   boundary document is the best answer available short of source, and it is specific
   enough to be checked against the app's actual behaviour.

## License and affiliation

Free for personal, non-commercial use. No redistribution, no commercial use, no warranty.
See [LICENSE](LICENSE).

**Not affiliated with Riot Games.** Yasuo Coach is not endorsed by Riot Games and does not
reflect the views or opinions of Riot Games or anyone officially involved in producing or
managing Riot Games properties. League of Legends and Riot Games are trademarks or
registered trademarks of Riot Games, Inc.

Using any third-party tool alongside League of Legends is your own decision and your own
responsibility under Riot's Terms of Service.
