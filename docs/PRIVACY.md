# Privacy

Short version: everything stays on your computer. There is no server.

## What the app sends

Nothing. While coaching a game the app makes no network requests at all — not to the
author, not to any analytics service, not anywhere. There is no account, no sign-in, no
licence check, no telemetry, and no crash reporting.

The only network access the app can ever make is from two optional commands you run
yourself:

- refreshing static patch data (items, champions, runes) from Riot's official static CDN,
  which is keyed only by champion, role and patch version — never by anything identifying
  you;
- fetching **your own** match history from Riot's public API, which requires you to supply
  your own API key in an environment variable, and which the app never stores or logs.

Neither runs unless you ask for it, and the app works without both.

## What the app stores, and where

Recorded games are written to a `sessions/` folder next to wherever the coach runs. A
session holds screenshots of your own screen taken during the game, the data read from
Riot's local endpoint, and a record of what the coach considered saying and what it said.

Two other folders under your user profile hold settings and the voice model:

```
%USERPROFILE%\.yasuo_mid_coach\
```

These are ordinary files. Nothing is encrypted, nothing is hidden, and you can read,
copy, or delete any of it at any time. Deleting a session folder deletes that game
completely.

Cached match data, if you use the optional replay half, is stripped of every other
player's identity before it is written to disk.

## What that means practically

- **Screenshots of your screen exist on your disk.** They are taken from your own display
  during a game, so anything else visible on screen at the time is captured too. Sessions
  can also grow large — plan for a few hundred MB per recorded game.
- **Nobody but you can see them** unless you share them yourself. If you attach a session
  to a bug report, you are choosing to publish those frames; check them first.
- **Uninstalling does not delete your sessions.** Delete the folders yourself if you want
  them gone.

## Contact

Privacy questions belong in an [issue](../../issues). There is no data to request or
delete, because none of it ever leaves your machine.
