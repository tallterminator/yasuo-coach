# What this tool refuses to do

You are being asked to run a closed-source binary alongside a game that bans third-party
software. This document is the specific version of "trust me" — specific enough that you
can check it against what the app actually does.

These constraints are copied from the project's internal contract, where they are marked
absolute and override any other instruction. They have not been softened for publication.

---

## Hard constraints

Yasuo Coach is a passive, observe-and-comment coach. It may use only information the
player can already see, and must never act on the player's behalf.

- **No game-memory reading.**
- **No packet sniffing or network interception.**
- **No client injection, DLL injection, or process hooking.**
- **No input automation** — it never sends a mouse or keyboard event to the game.
- **No inference of hidden state** — no fog-of-war enemy positions, no timers the player
  cannot see, no exact enemy ability or ultimate cooldowns.

These are not preferences. They are the line between a coach and scriptware, and the
project treats crossing them as a defect regardless of how useful the result would be.

### Why an enemy cooldown tracker will never be added

It is the single most requested feature of tools like this, and it is permanently out of
scope. Riot's third-party compliance surface prohibits products that provide previously
unknown session-specific information, and names enemy cooldown tracking specifically. A
tool that tracks what you could not see is not observing your game; it is playing part of
it for you.

The coach may ask you to *notice* an uncertainty without resolving it — "does Zed have
Flash?" — but only when it supplies no answer, no availability status, no timer, and no
prediction, and only when the question was not triggered by an enemy spell use or any
stored enemy-availability state. "Is Zed's Flash up?" and "Zed has Flash down" are both
prohibited, because both assert or orchestrate tracking.

### Visible-camp boundary

"Their red is missing" is permitted only after your own screen has visibly confirmed that
camp is empty. No unseen jungler location or route is ever inferred.

---

## Allowed data sources, in full

While coaching a live game, exactly two:

1. **Riot's official Live Client Data API** at `https://127.0.0.1:2999` — Riot's sanctioned
   local endpoint, which exposes only player-visible data and answers only while a game is
   running on your machine.
2. **Your own screen** — screenshots of your own display.

**No live game reasoning ever uses a network source.** All live coaching is
visible-state-only from those two local sources.

Three further sources exist outside the live path, all optional and all off by default:

- **Static patch reference data** — items, champions, runes and summoner spells from
  Riot's official static CDN (`ddragon.leagueoflegends.com`) and Community Dragon, fetched
  only by an explicit command, keyed only by champion, role and patch version. Never
  keyed by a player name or a match id, never fetched during a game, and the app works
  without it.
- **Your own match history** — the Riot Web API, fetched only by an explicit command using
  a personal API key you supply in an environment variable. It is never stored and never
  logged. Only your own account's records are fetched, and results are stripped of every
  other player's identity before anything is written to disk.
- **Offline statistics import** — a locally downloaded, permissively licensed dataset,
  parsed by an explicit command. This one fetches nothing at all.

Nothing else.

---

## How directly the coach speaks — decided

The project separates *technical* constraints, listed above and settled, from *how
directly the coach phrases advice*.

The coach gives strategic direction out loud — "recall now", "don't take this fight",
"the window to force a fight is closed" — and it names an opponent's visible loadout
(the spells and items drawn on the scoreboard). It never gives a mechanical input
command ("press Q now", "flash now"), and it never claims whether an opponent's spell or
ability is available or when it comes back, because that is the hidden-state line above.

That posture was first allowed for the author's private use (2026-09-02) and, on
2026-09-05, confirmed for every build the author shares or releases. The reasoning is
recorded in the project: Riot's compliance surface distinguishes products that dictate
decisions from products that highlight them, and the author decided that a coach telling
you what it would do — while reading nothing you cannot see and touching nothing — is the
tool they want to run and to hand to friends. Whether to run any third-party tool
alongside League of Legends is your own decision under Riot's Terms; see the README.

None of the hard constraints above move with this: it is a question about wording, not
about what the tool reads.

---

## How to check any of this

The app records what it did. Every session is a folder of ordinary files on your disk
containing what it read, what it considered saying, and what it actually said, with the
evidence attached to each line. `coach doctor` reports which sources are reachable and
which screen regions it believes it can read.

If you find the app doing something this document says it does not, that is a bug worth
[reporting](../../issues) — and a serious one.
