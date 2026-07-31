# Loot Tracker

Builds a searchable SQLite database of *where items come from and what they are*
on Aardwolf — what mobs drop, what room objects yield, what shops natively stock,
and the full stat block of anything you identify — captured automatically as you
play. Item tracking for everything you loot. Works entirely on its own; no other
plugin required.

## Requires

- the Aardwolf client package: `constants.lua`, `gmcphelper`, bundled `sqlite3`,
  `json` (the share wire format), and `aardwolf_colors` for item colours
- `async` for the shared-API calls — optional, and only touched once you run
  `loot auth`. Everything else works without it.

## What it captures

Everything is stamped with the zone / room id / room name from GMCP at the time.

| source | trigger | table |
|---|---|---|
| **mob loot** | `You get <item> from the corpse of <mob>` | `loot` |
| **room resources** | `You get <item> from <bush/chest/...>` | `gathered` |
| **shop stock** | a `list` row whose Qty is the unlimited marker | `shop_stock` |
| **floor spawns** | `loot here <keyword>` (picks it up, records full name) | `gathered` |
| **item stats** | any `id` / `appraise` box that scrolls past | `item_stats` |

- **Gold** and **player corpses** (PK loot) are never recorded.
- **Item colours** are captured (`item_color`) so a front end can show them as in
  game.
- **Your own bags are never mistaken for room objects.** It reads your carried
  containers from `invdata` (items of type 11) and skips gets from them — so it
  keeps working no matter what you rename a bag to. A configurable word-list
  (`loot bags`) is a fallback.

It does **not** depend on Search & Destroy or any mapper: the mob→room mapping
comes straight from your own loots via GMCP, so it stands alone.

## Item name variations

Two things the game welds onto item names would otherwise each create their own
row, so they're stripped for **`loot` and `gathered`** — the tables that answer
*where does this come from*:

- **Stacked pickups.** `You get 2 * the body armor of Sycorax's minions` is the
  same item as a single one, so the `N * ` count goes.
- **Bonus-loot flags** (see `help gameload`). A mob-reset item can roll random
  stats and gains a flag that becomes part of its name — `(Polished) Runed Jade
  Bracer`. The flag is a per-kill dice roll, not a different item, so the base
  name is what gets recorded.

Stripping any leading `(Word)` would wreck real names, though: `(Seekers) Elixir
of Free Movement`, `(Aarchaeology) ...`, and a shop that genuinely sells
`(Legendary) Bracers of the Warrior-Poet`. So **two signals must agree** — the
word is a known flag *and* the bracket colour matches its tier:

| brackets | tier |
|---|---|
| `@W((@RGodly@W))` | Godly |
| `@Y(@W…@Y)` | Divine, Mythical, Fabled, Epic, Legendary |
| `@R(@W…@R)` | Eternal, Exalted, Majestic, Wondrous, Radiant |
| `@G(@W…@G)` | Brilliant, Dazzling, Shimmering, Gleaming, Sparkling |
| `@G(@W…@G)` | Vibrant, Shiny, Burnished, Enhanced, Polished |

That colour test is what saves the shop item: its Legendary is `@W(@MLegendary@W)`
— white brackets, magenta word — so it's left alone. `loot flags` lists the set and
`loot flags add|del <word>` edits it.

**`shop_stock` and `item_stats` are deliberately left alone.** Shops sell fixed
inventory and bonus rolls only happen on mob-reset loot, so a `(...)` in a shop
name is always real. And a bonus item's stats are *inflated* — filing them under
the base name would poison the base item's stat block with one lucky roll.

`loot dedupe` retro-fixes rows captured before this existed: names are rewritten to
their base form, and where that collides with an existing row the counts are folded
together and the variant dropped. Corrected rows go unsynced so the pool gets the
fixed version. It can't retract variant rows already *in* the pool — the API has no
delete path — it just stops new ones going up.

## Item stats

Where the other tables answer *where did this come from*, `item_stats` answers
*what is it*. Every `id` or `appraise` box that scrolls past your screen gets
parsed and stored — level, type, worth, weight, score, wearable slot, material,
flags, keywords, the `Stat Mods` block (str/int/wis/dex/con/luck, hp/mana/moves,
hit/dam), the `Resist Mods` block (all physical, all magic, and each element),
weapon data (type, average damage, specials, inflicts, damage type), castable
spells, portal destinations and container capacity.

To fill a whole shop at once, `appraise 1-12` walks that range of `list` numbers,
one per second. The alias matches **only** digits-dash-digits, so `appraise 4`,
`appraise sword` and even `appraise 2-handed sword` go to the game exactly as
typed. It stops if you go AFK partway, caps at 60 items so a typo can't fire off
hundreds of commands, and `loot appraise stop` halts it.

Two things worth knowing:

- **This is passive.** It reads boxes already on your screen and sends nothing.
  A shop `appraise` fills the DB with no `IDENTIFY WISH` and no extra traffic.
- **`loot autoid on`** additionally sends `id` for you on each fresh corpse
  drop. It's **off by default** because it needs the identify wish to return
  anything useful. It skips items already fully appraised, identifies by item
  serial from `invdata` (names aren't keywords), and — like every other send
  this plugin makes — never fires while you're flagged AFK.

Records captured without the wish are flagged `full_id = 0` and `loot stats`
marks them *partial*, so it's easy to re-identify them later.

## Use

| command | what |
|---|---|
| `loot` | row counts |
| `loot item <text>` | where an item comes from (mob / zone / room) |
| `loot mob <text>` | what a mob drops and where it was |
| `loot shop <text>` | which shops natively stock an item |
| `loot gathered <text>` | where a room resource is picked up |
| `appraise <a>-<b>` | appraise a range of shop list numbers, paced |
| `loot appraise stop` | halt a running bulk appraise |
| `loot stats <text>` | stat block for an item (level, mods, resists, weapon) |
| `loot id <keyword>` | identify an item and store its stats |
| `loot autoid on\|off` | auto-identify fresh drops (needs identify wish, off by default) |
| `loot idnote on\|off` | confirm each stat capture on screen (on by default) |
| `loot here <keyword>` | get a floor spawn and record its full name/colour |
| `loot del <item> [roomid]` | remove matching rows (roomid scopes to one room) |
| `loot bags [add\|del <word>]` | container words ignored for gathers |
| `loot flags [add\|del <word>]` | bonus-loot flags stripped from names |
| `loot dedupe` | collapse variant rows already captured |
| `loot recent` | last 15 things looted |
| `loot resync yes` | forget sync flags; re-send everything next upload |
| `loot clear loot\|shop\|gathered\|stats` | wipe a table |
| `loot help` | command list |

## Sharing (pool data with your clan)

Both paths merge the same way as the local capture — new rows added, existing
rows kept — and neither sends anything over a game channel.

**Files (offline):**

- `loot export [file]` — dump the whole DB to a text file.
- `loot import <file>` — merge someone else's export.

Pass the file around out-of-band (Discord, git, a shared drive).

**Shared API (online):**

The endpoint is built into the plugin (not user-settable). `loot api` shows it
and your auth status.

- `loot auth` — register this character once. Sends your GMCP character name (or
  asks for one), the service returns an auth key, and the plugin stores it
  **hidden** — you never see or handle the key.
- `loot upload` — push new/changed rows into the shared pool (needs auth).
  `loot upload full` re-declares everything.
- `loot update` — pull the pooled data down and merge it (needs auth).

Nothing contacts the API until you run `loot auth`, so it stays quiet until you
opt in. The service itself is a separate project.

**Only new work is sent.** Every row carries a local `synced` flag. A row is
uploaded once, then skipped; touching it again (a fresh loot bumping `count`, a
shop reprice, a better identify) clears the flag so the updated version goes next
time. Rows pulled down with `loot update` land already-synced, so nobody's data
bounces back to the pool. Rows merged from a clanmate's *file* land unsynced,
since the pool hasn't necessarily seen those.

The practical effect: the first sync ships everything, and after that a session's
worth of new drops is a few hundred bytes instead of the whole database. `loot`
shows the backlog, `loot api` breaks it down per table, and `loot resync yes`
clears every flag to force a full re-send (for when the pool was wiped
server-side and the local flags no longer reflect reality).

Uploads go out in slices capped at 32KB of encoded JSON — a failed batch stops
there, reports how many rows landed, and re-running resumes exactly where it
stopped. Nothing is marked synced until the server confirms that batch.

**Once a week the whole DB is re-declared** (`loot upload full` forces it now).
The pool tracks how many players have each row and learns that from what you
declare, so a pure delta would never register you on the items you already share
with everyone. The full pass goes out in the same size-capped batches, and the
timestamp is only recorded once it completes, so an interrupted one retries.
`loot api` shows when the last one ran.

### Why uploads are capped at 32KB

A single ~144KB POST never arrived: the server saw the headers, waited for a body
that stopped partway, and the request died at the client timeout. Keeping every
request well under one socket buffer avoids it, which is what the 32KB cap is for.

Worth recording, since the obvious fix is wrong here: the usual culprit is
LuaSocket's sinks calling `sock:send(chunk)` without looping, because `send()`
returns the *index of the last byte written* rather than a count. **That does not
apply on MUSHclient's HTTPS path.** MUSHclient doesn't use stock LuaSec — it ships
a BIO-based shim (`lua/ssl.lua`) whose `send` is `bio:write(m) and bio:flush()`,
all-or-nothing, and which returns a flag rather than a byte index. Wrapping it in
a `while sent < #data` loop makes `sent` never advance, so it re-sends the same
tail until the peer closes the connection — a 25KB batch fails with `closed`.

So don't "fix" `lua/socket.lua` for this. The root cause on this transport is
still unconfirmed (the body also crosses a thread boundary via llthreads before
LuaSocket sees it); the size cap sidesteps it.

Database: `SolaoLoot.db` in the world-files directory. Window-less — it's capture
triggers plus a SQLite DB and the `loot` query commands.
