# Loot Tracker

Builds a searchable SQLite database of *where things come from* on Aardwolf —
what mobs drop, what room objects yield, what shops natively stock, and your
campaign goal targets — captured automatically as you play. Works entirely on its
own; no other plugin required.

## Requires

- the Aardwolf client package (uses `constants.lua`, `gmcphelper`, bundled
  `sqlite3`, and `aardwolf_colors`)

## What it captures

Everything is stamped with the zone / room id / room name from GMCP at the time.

| source | trigger | table |
|---|---|---|
| **mob loot** | `You get <item> from the corpse of <mob>` | `loot` |
| **room resources** | `You get <item> from <bush/chest/...>` | `gathered` |
| **shop stock** | a `list` row whose Qty is the unlimited marker | `shop_stock` |
| **campaign goals** | `You still have to kill * <mob> (<area>)` | `targets` |
| **floor spawns** | `loot here <keyword>` (picks it up, records full name) | `gathered` |

- **Gold** and **player corpses** (PK loot) are never recorded.
- **Item colours** are captured (`item_color`) so a front end can show them as in
  game.
- **Your own bags are never mistaken for room objects.** It reads your carried
  containers from `invdata` (items of type 11) and skips gets from them — so it
  keeps working no matter what you rename a bag to. A configurable word-list
  (`loot bags`) is a fallback.

It does **not** depend on Search & Destroy or any mapper: the mob→room mapping
comes straight from your own loots via GMCP, so it stands alone.

## Use

| command | what |
|---|---|
| `loot` | row counts |
| `loot item <text>` | where an item comes from (mob / zone / room) |
| `loot mob <text>` | what a mob drops and where it was |
| `loot shop <text>` | which shops natively stock an item |
| `loot gathered <text>` | where a room resource is picked up |
| `loot goals [text]` | campaign/quest goal targets (mob → area) |
| `loot here <keyword>` | get a floor spawn and record its full name/colour |
| `loot del <item> [roomid]` | remove matching rows (roomid scopes to one room) |
| `loot bags [add\|del <word>]` | container words ignored for gathers |
| `loot recent` | last 15 things looted |
| `loot clear loot\|shop\|gathered\|goals` | wipe a table |
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
- `loot upload` — push your DB into the shared pool (needs auth).
- `loot update` — pull the pooled data down and merge it (needs auth).

Nothing contacts the API until you run `loot auth`, so it stays quiet until you
opt in. The service itself is a separate project.

Database: `SolaoLoot.db` in the world-files directory. Window-less — it's capture
triggers plus a SQLite DB and the `loot` query commands.
