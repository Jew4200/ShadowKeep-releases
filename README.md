# ShadowKeep

Keeps **NVIDIA ShadowPlay** turned on. When any app, driver update, or screen
recorder switches ShadowPlay (or Instant Replay) off, ShadowKeep switches it
back — automatically, within about a second — so your capture doesn't quietly
die the moment something else grabs the GPU.

**[⬇ Download the latest release](../../releases/latest)** · one `ShadowKeep.exe`,
no installer, no admin rights, nothing else to install.

---

## At a glance

| | |
|---|---|
| **What it does** | Watches NVIDIA's ShadowPlay state and restores it whenever something turns it off |
| **How** | Reads and corrects NVIDIA's own registry setting. **No injection, no hooking** of any other process |
| **Install** | None. A single portable `ShadowKeep.exe` in the system tray |
| **Platform** | Windows 11 / Windows 10 with an NVIDIA GPU and the NVIDIA App (or GeForce Experience) |
| **Admin rights** | Not required. Runs as you, writes only your own per-user settings |
| **Telemetry** | None. It makes no network calls |
| **Footprint** | One tray icon and a log file next to the exe |

---

## Why it exists

ShadowPlay doesn't always stay on. Some screen recorders (Gyazo is a common
one) tell NVIDIA to release the capture when *they* record, which switches
ShadowPlay off and leaves it off. Driver updates and some games' anti-capture
flags do the same. If you rely on Instant Replay to catch the last few minutes
of whatever just happened, you find out it was off only after you needed it.

ShadowKeep closes that gap. You turn ShadowPlay on once; ShadowKeep keeps it on.

## How it works

NVIDIA stores ShadowPlay's state in the registry, and for each toggle it keeps
**two** values: the *live* flag (what's active right now) and a *User* flag
(what **you** last chose in NVIDIA's own UI). ShadowKeep watches that key and,
the instant a live flag drifts away from your User flag, writes it back to match
**your** choice.

Because the target is always NVIDIA's own "what the user chose" value:

- If **you** turn ShadowPlay off in the NVIDIA App, ShadowKeep follows you — it
  never fights a change you made yourself.
- If **anything else** turns it off, ShadowKeep restores it.

It never reads, writes, or touches any other program. It only corrects NVIDIA's
own setting, which is why it keeps working across driver updates and can't break
anything.

**One honest limitation:** it's restore-*after*, not block-*before*. There's a
sub-second window between something turning capture off and ShadowKeep turning
it back on. For keeping your session recording that's fine; it is not a promise
that zero frames are ever missed.

## Using it

Double-click `ShadowKeep.exe`. It sits in the system tray (shield icon —
expand the hidden-icons `^` arrow if you don't see it). Right-click for:

- **Status** — current ShadowPlay state and whether ShadowKeep is enforcing.
- **Keep ShadowPlay on (enforce)** — master switch. Uncheck to pause ShadowKeep.
- **Start with Windows** — run at logon (per-user, no admin, no service).
- **Show recent activity** — every restore, with the app that was in the
  foreground at the time.
- **Exit**.

## Verifying a download

Each release ships `ShadowKeep.exe` and `ShadowKeep.exe.sha256`. Check it with:

```powershell
Get-FileHash .\ShadowKeep.exe -Algorithm SHA256
```

and compare against the `.sha256` file.
