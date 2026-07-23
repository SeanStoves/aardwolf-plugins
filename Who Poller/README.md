# Who Poller

Sends `who` on a timer and eats the reply so it never reaches your screen. Built
to keep [Player Tracker](../Player%20Tracker/README.md) fed without a wall of
text every ten minutes, but it doesn't depend on it — anything that triggers off
`who` gets the same benefit.

## Requires

- the Aardwolf client package (uses `constants.lua` and `serialize`)

## How the gag works

A catch-all trigger is armed **only** for the length of the reply: it goes on
right before the `who` is sent, and off as soon as the `Players invis:` footer
and the blank line after it have gone by. A safety timer (8s) releases it anyway
if the reply never arrives, so a dropped `who` can't leave your output swallowed.

Gagging hides a line from the output window — it does **not** stop triggers. Every
other plugin still sees all of it, which is exactly why Player Tracker keeps
filling up while you see nothing.

## AFK

Polls are skipped while you're flagged AFK (the `AFKMode` variable existing).
Auto-sending a command every N minutes while you're away is anti-idle, which
Aardwolf's rules don't allow — gating on the AFK flag keeps this an assist for
when you're actually at the keyboard. Turn it off with `whopoll afk off` if your
setup doesn't need it.

## Use

| command | what |
|---|---|
| `whopoll` | status — interval, AFK skip, poll count, gag state |
| `whopoll on` / `whopoll off` | start / stop polling |
| `whopoll <minutes>` | set the interval (1–120, default 10) |
| `whopoll now` | poll once immediately |
| `whopoll afk on\|off` | skip polls while AFK (default on) |

Settings persist across sessions. The default 10-minute interval lines up with
Player Tracker's 10-minute online window: every poll refreshes everyone still on,
so anyone who ages out has actually gone.
