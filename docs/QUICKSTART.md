# Quickstart — setting the coach up on a fresh computer

Getting from a clean Windows install to a coached game. About 15 minutes, most of it a
download.

---

## What you need

| | |
| --- | --- |
| **Windows** | Windows-only — it captures your screen and speaks through Windows audio. Developed and tested on Windows 11; Windows 10 should work but is unverified. |
| **League of Legends** | Installed. The coach reads Riot's official Live Client Data API on `127.0.0.1:2999`, which is on by default. |
| **Disk** | ~250 MB: the app is ~120 MB, the bundled voice model ~63 MB, and recorded sessions grow from there. |
| **Speakers or headphones** | Not optional. `coach play` **refuses to start a game it cannot be heard in** — see step 2. |
| **WebView2** | Ships with Windows 11. On Windows 10 the installer fetches it for you. |

Nothing else. No Python, no admin rights, no account, no API key for the live coach, and
no separate voice download: the voice is inside the installer.

---

## Step 1 — Install the app

Download the newest release from the public **Releases** page
(`github.com/tallterminator/yasuo-coach/releases`). Every release carries two files,
built from a clean checkout and checked with `coach doctor` before they were published:

- **`YasuoCoach-setup.exe`** — the installer. Run it; it installs under your user profile
  (no admin prompt) and adds a Start Menu entry.
- **`YasuoCoach-portable.zip`** — the same app as a folder. Unzip anywhere and run
  `YasuoCoach.exe` from inside it. The folder is self-contained; `YasuoCoach.exe` finds
  `coach.exe` beside itself, so keep the two together.

Windows may show a SmartScreen prompt because the installer is unsigned: choose
**More info**, then **Run anyway**.

**If someone sent you `YasuoCoach-setup.exe` directly**, run it the same way. It needs
Windows 10 or 11 and the League client on the same PC, nothing else. The coach only reads Riot's local Live Client Data API on your own machine
and sends nothing anywhere. Recordings of your games stay in a `sessions/` folder on your
own disk and are large — about 3 GB per 30-minute game — so keep an eye on the drive.
**Sessions → Disk** measures what they occupy and reclaims the screenshots of older
games on request, keeping everything grading and analysis read; or delete the folder
yourself whenever you like, since the coach never needs it again.

Both give you the same screens: **Play**, **Sessions**, **History** and **Settings**,
plus a developer **Live** dashboard that is off until you turn it on in Settings.

> Recorded games are written to a `sessions/` folder next to wherever the coach runs, so
> put the portable folder somewhere you can write to — not `C:\Program Files`.

---

## Step 2 — Check you can hear it

The voice (`en_US-amy-low`, a Piper neural voice) is bundled with the app since
2026-09-01, so there is nothing to download. What you must have is an audio device: at
the start of every game `coach play` speaks one calibration line and **exits if it
cannot be heard** — deliberately, since a game recorded around a silent coach is a game
you cannot grade and cannot replay.

**No speakers, or want to skip this for now?** Run with `--no-speak` (the Play screen has
a toggle). You get every coaching line as text and no audio, and the voice check is
skipped entirely.

**Want a different voice?** Drop both files of any English Piper voice — the model and
its JSON config, which must sit together and share a name — into
`%USERPROFILE%\.yasuo_mid_coach\voices\`:

```
en_US-amy-low.onnx          (~63 MB)
en_US-amy-low.onnx.json     (~4 KB)
```

That folder is searched **before** the bundled copy (as is the older
`%USERPROFILE%\.lol_live_coach\voices\`), so a voice you supply wins. Piper voices are
published at **huggingface.co/rhasspy/piper-voices**, arranged by language
(`en/en_US/amy/low/…`).

> **Both files, or neither works.** The engine loads the `.onnx` and then reads the
> `.onnx.json` beside it. A model without its config fails at load with an unhelpful error.

---

## Step 3 — Run the preflight

Open the app and go to the **Play** screen: the six checks run themselves and sit above the
Start button. **Settings → Preflight** has the same panel with a Check button, and from the
folder holding `coach.exe` the terminal form still works:

```powershell
.\coach.exe doctor
```

Six checks, wherever you read them. Here is what the answers mean:

| Check | What you want | If it isn't that |
| --- | --- | --- |
| **build** | `frozen coach.exe built <date>` | A source checkout prints its commit instead. Every session records this line, so a stale exe can be told from the tree. |
| **platform** | `Windows` | The coach does not run elsewhere. |
| **display** | Names which HUD regions are measured | See below — usually fine. |
| **live client data** | Timeout is **normal** with no game running | Re-run during a match to confirm. |
| **voice** | `neural: en_US-amy-low.onnx` | The bundled voice is missing or a `voice.json` is malformed — reinstall, or see step 2 to supply a voice. |
| **webview2** | A version number | Install the Edge WebView2 runtime. |

**About `display`.** The coach works at any resolution and aspect ratio, and the check
tells you how much of the screen reading is verified there:

- **1920×1080** — every HUD rectangle was measured on real frames.
- **2560×1440** — the minimap and recall bar are measured; the ability slots are
  **scaled from 1080p**.
- **Anything else** (4K, 1600×900, ultrawide, 16:10, …) — every rectangle is scaled from
  the 1080p measurement, re-anchored to the corners League anchors its HUD to.

"Scaled" means the 1080p rectangle resized, not a frame anyone has checked at your size,
so the coach labels those reads as unverified rather than pretending they were measured.
It costs you very little either way: the live cues come from the Live Client Data API,
which is resolution-independent, and ability-slot reading is replay-only. Play at 1080p if
you want the fullest verified capture; otherwise ignore it.

---

## Step 4 — A smoke test, then your first game

**Before a real game**, launch the app, press **Start** on the Play screen and then **End
session** with no game running. You should hear one calibration line, see `Playing.`, and
then `SESSION CLOSED`. That is two minutes that proves audio, the process boundary and
session finalisation all work.

Then start a game and press **Start**. The coach will:

1. Speak one calibration line and refuse to continue if you cannot hear it.
2. Record the session, watch the game, and speak coaching lines as it goes.
3. On **End session**, shut down in order so the recording is finalised with an `ended_at`
   stamp.

**Always use End session** rather than closing the window. The ordered teardown is what
writes the closing stamp; a session without one has to be repaired afterwards with
`coach close-session <path>`.

---

## Step 5 — Grade what it said

This is the part that makes the coach better, and it is the project's own gate.

In the app: **Sessions**, pick the game, **Grade this game**. Every line it spoke appears
with its clock, and you answer two questions per line — was it fair, was it useful. A
wrong click is not permanent: **Change** reopens an answer and records a correction beside
the first one. **Grading status** and **Assemble grades** are at the bottom of the list.

The terminal form is `.\coach.exe grade sessions\<your-session>`, and prints
the review sheet the app ships with as the sheet to grade against.

Nothing about the coach can be tuned honestly until cues are graded, because until then
nobody has told it which of its lines were worth hearing.

---

## Optional — the replay half

Separate from the live coach and never touching it. It reads your finished games from
Riot's public match API to find patterns across games.

It is the **History** screen: paste a personal API key from `developer.riotgames.com` into
the box, press **Sync my games**, then read the results under "Across your own games". The
key is held in that window only — never written to disk — and cleared once a sync succeeds.

The same thing from a terminal:

```powershell
$env:RIOT_API_KEY = "RGAPI-..."
.\coach.exe sync --riot-id "GameName#TAG" --count 20 --timelines --queue 420
.\coach.exe analysis baseline
```

Cached match data is stored outside the repository and stripped of every other player's
identity before it is written.

---

## Optional — tuning the voice

Create `%USERPROFILE%\.yasuo_mid_coach\voice.json`:

```json
{
  "engine": "neural",
  "length_scale": 0.78,
  "volume": 1.0
}
```

- `engine` — `"neural"` (Piper, the default) or `"sapi"` (the Windows built-in voice,
  which needs no model file).
- `length_scale` — pace, where **lower is faster**. Default `0.85`. Use `~0.78` if the
  coach talks over itself: a shorter utterance is a narrower window in which a second cue
  gets dropped.
- `volume` — `0.0` to `1.0`.

> A malformed `voice.json` makes the coach **fail loudly rather than fall back to
> defaults**. That is deliberate — a typo that left it sounding exactly as before, with no
> sign the file had been read, is the one outcome worth refusing. `coach doctor` reports
> the problem without your having to start a game.

---

## Troubleshooting

| Symptom | Cause |
| --- | --- |
| `no Piper voice model found` | The install is incomplete (the bundled `voices\` folder is missing) or a voice you supplied is in the wrong folder. Reinstall, or see step 2. |
| Voice check fails and `play` exits 1 | The coach cannot be heard. Fix audio, or run `--no-speak`. |
| Voice reported OK but nothing is audible | Check the Windows output device and volume mixer. |
| `live client data ... timed out` | Expected with no game running. Only a problem during a match. |
| `doctor` says regions are "scaled from 1080p" | Nobody has measured the HUD at your resolution, so the 1080p rectangles are resized. Correct behaviour — see step 3. |
| A session has no `ended_at` | It was closed by killing the window. Repair with `coach close-session <path>`. |
| The app opens but the Live screen is blank | The dashboard serves on `127.0.0.1:7878` only once a game is running. |
