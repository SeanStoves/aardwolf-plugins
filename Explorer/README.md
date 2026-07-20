# Explorer

Builds a map of each Aardwolf area **from GMCP as you walk it** — records every
room's name and exits, tracks how much of the current area you've covered, shows
which exits still lead somewhere new, and exports a **mermaid** flowchart (a
gaardian-style map doc) plus raw **JSON** per area.

It only records the rooms you walk into — it never moves your character.

## Requires

- the Aardwolf GMCP handler (`room.info`)

## Window

Four buttons + a live footer:

| Button | Does |
|--------|------|
| **Export Mermaid map** | Write the current area's map to `map_<Area>.md` (a mermaid flowchart). |
| **Export JSON** | Write the current area's rooms/exits to `map_<Area>.json`. |
| **Show unexplored exits** | List the rooms here that still have exits to rooms you haven't recorded. |
| **List recorded areas** | Every area you've mapped, with room counts. |

Footer: current **Area**, **Rooms** recorded, **Unexplored exits** remaining
(exits pointing at a room you haven't stepped into yet — walk those to finish the
area).

Files are written to the plugin's own folder. Drag by the title bar, right-click
to hide, type `explore` to summon it.

## Commands (same as the buttons)

```
explore                 show/hide the window
explore mermaid | json  export the current area
explore new             rooms here with unexplored exits
explore list            areas recorded
explore clear           forget the current area (then 'explore clear yes')
explore help            this
```

## Mermaid output

Each room is a node (`r<roomid>["Room Name"]`); each exit is a labeled edge
(`r100 -->|n| r101`). Exits to rooms you haven't entered show as `?` nodes, so the
gaps in your coverage are visible on the map. Paste the file's contents anywhere
mermaid renders (GitHub, many editors) to see the zone.

## Workflow

Walk an area normally. When "Unexplored exits" hits 0 you've been everywhere the
mapper can see. Click **Export Mermaid map** to save the diagram. Your data
persists across sessions and accumulates as you explore.
