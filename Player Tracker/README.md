# Player Tracker

Watches every `who` you run and remembers everyone it sees — one record per
player, keyed on name, updated in place (no duplicates). For each player it keeps
their clan, ranks, level box, current title and a last-seen timestamp, and shows
it all in a searchable miniwindow.

## Requires

- the Aardwolf client package (uses `movewindow`, `constants.lua`, the z-order
  monitor, and `serialize`)

## What it captures

- **Every clan and the unclanned.** The `who` list decorates each clan
  differently (`|| BOOT ||`, `}HooK{`, `[Ba'al]`, `|Watchmen|`, …); the plugin
  reads those tags to label the clan, and still records players with no clan at
  all. (Older builds only caught one clan — hence the "stuck at 30" bug.)
- Leading `who` flags — `*AFK*`, `(OPK)`, `(HARDCORE)`, `(Linkdead)` — and rank
  brackets (`[Advisor]`, `[Council]`, …) are peeled off so the name and title
  come out clean.
- `Players found: N` feeds a running **average online** stat.

## Online vs offline

Aardwolf only tells you when someone logs in or out for **your own clan**, so
that's what drives the live dot for clanmates (Boot's `is in attendance` /
`has had enough of learning` notices). Everyone else is marked online for a while
after the last time a `who` saw them, then decays to offline.

## Use

- **`pt`** — show/hide the window (defaults to on top).
- **Click a name** — prints that player's full record.
- **Click the search bar** — filter the list by name, clan or title (click the
  `x` to clear).
- **Mouse-wheel** — scroll the list.
- **Drag the bottom-right grip** — resize.
- **Right-click the title bar** — Bring to Front / Send to Back / toggle Always
  on Top / Search / Clear filter / Hide.

Commands (the window is the main UI; these are extras):

| command | what |
|---|---|
| `pt find <text>` | search name / clan / title, print matches |
| `pt info <name>` | full record for one player |
| `pt online` | list who's online now |
| `pt count` | quick totals |
| `pt export` | dump `players.csv` (gitignored) |
| `pt clear yes` | wipe the database |
| `pt help` | command list |

The database, window size/position and stats persist across sessions.
