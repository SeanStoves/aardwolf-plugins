# Explorer

Generates map docs straight from the **Aardwolf mapper's own database**. The
mapper already knows every room it has seen — name, exits, terrain, flags, notes,
coordinates — so Explorer reads that SQLite DB read-only and emits a **mermaid**
flowchart (a gaardian-style map doc) plus raw **JSON**, for the area you're in, a
named area, or every area at once. No re-walking; it uses what the mapper already
recorded.

It reads the DB read-only — it never moves you or changes the map.

## Requires

- the Aardwolf GMCP mapper (`b6eae87ccedd84f510b74714`) and its database
  (`Aardwolf.db` in the MUSHclient folder, by default)

## Window

| Button | Does |
|--------|------|
| **Mermaid map (this area)** | Write the current area's map to `map_<Area>.md`. |
| **JSON (this area)** | Write the current area's rooms + exits to `map_<Area>.json`. |
| **Export ALL areas** | A mermaid file per area the mapper knows (many files). |
| **List areas in DB** | Every area + room count. |

Footer shows the current **Area** and the last action. Files land in the plugin
folder. Drag by the title bar, right-click to hide, type `explore` to summon it.

## Commands

```
explore                 show/hide the window
explore mermaid | json  the area you're in
explore area <name>     map a named area (partial name ok)
explore all             every area (many files)
explore list [filter]   areas in the DB
explore help            this
```

## Mermaid output

- Each room is a node showing **name + `#roomid`**, plus its **info flags**
  (`shop`, `bank`, `pk`, `safe`, …) and any **notes** the mapper stored.
- **Solid** edge `r100 -->|n| r101` = a normal direction; **dotted**
  `r100 -.->|enter portal| r250` = a special/warp exit.
- Exits leaving the area show the destination as a `?<br/>#id` stub, so you can
  see where an area connects out.
- Nodes are **colored by terrain** (forest, ocean, cave, …), gaardian-style.
- Keyed by room **id**, so identical maze rooms stay distinct — follow the edges.

Paste a file's contents anywhere mermaid renders (GitHub, many editors). The JSON
carries the full room + exit data (names, terrain, flags, notes, coords) for
building richer pages.

## Note

Reads whatever the mapper has walked. Areas you've never visited won't be in the
DB — walk them once (the mapper records automatically) and they're available.
