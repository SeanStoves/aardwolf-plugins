# Explorer

Builds a map of each Aardwolf area **from GMCP as you walk it** — records every
room's name and exits, tracks coverage, and exports a **mermaid** flowchart (a
gaardian-style map doc) plus raw **JSON** per area.

It records the **actual command you type** to move, so warps and special exits
(`enter portal`, `pull lever`, a maze word) become real edges labeled with the
command — which makes even mazes followable: each edge tells you exactly what to
type to make that move. You can also annotate rooms with notes.

It only records where you go — it never moves your character.

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
explore note <text>     note the room you're in (shows on the map)
explore note            show this room's note
explore new             rooms here with unexplored exits
explore list            areas recorded
explore clear           forget the current area (then 'explore clear yes')
explore help            this
```

## Mermaid output

Each room is a node (`r<roomid>["Room Name"]`); each move is a labeled edge:

- **Solid** `r100 -->|n| r101` — a normal direction.
- **Dotted** `r100 -.->|enter portal| r250` — a warp or special command (anything
  that isn't a plain direction). The label is the exact command you typed to make
  the move, so you can follow it.
- Rooms you haven't entered show as `?` nodes, so coverage gaps are visible.
- Room notes render under the name in the node.

Because nodes are keyed by room **id**, identical-looking maze rooms are distinct
and the graph is the true connectivity — follow the edges, not the geography.
Paste the file's contents anywhere mermaid renders (GitHub, many editors).

## Workflow

Walk an area normally. When "Unexplored exits" hits 0 you've been everywhere the
mapper can see. Click **Export Mermaid map** to save the diagram. Your data
persists across sessions and accumulates as you explore.
