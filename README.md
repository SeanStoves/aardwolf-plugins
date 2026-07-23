# aardwolf-plugins

MUSHclient plugins for the [Aardwolf](https://www.aardwolf.com/) MUD.

They run inside the [Aardwolf MUSHclient client package](https://github.com/fiendish/aardwolfclientpackage)
(they use its bundled `movewindow`, `serialize`, and `constants.lua`). Each
plugin lives in its own folder with a README and a screenshot. To install one:
MUSHclient → **File → Plugins → Add…** and pick the plugin's `.xml`. Nothing
depends on the folder a plugin lives in.

## Plugins

- **[Recall Manager](Recall%20Manager/README.md)** — a miniwindow of clickable
  buttons you build yourself. Each button fires a `|`-separated list of commands
  (recalls, portals, held-item transports, shop runs, whatever you repeat), with
  optional arrival detection and use counters. Ships empty and fully
  user-configurable; nothing about your character is hardcoded.
- **[Explorer](Explorer/README.md)** — generates map docs from the Aardwolf
  mapper's own database: reads the mapper's rooms/exits/terrain/flags/notes and
  exports a gaardian-style terrain-colored **SVG grid map** (+ JSON) for any area.
  Read-only; never moves you or changes the map. Companion library:
  [SeanStoves/aardwolf-maps](https://github.com/SeanStoves/aardwolf-maps).
- **[Info Window](Info%20Window/README.md)** — captures `INFO:` lines into their
  own draggable, resizable, scrollable miniwindow (keeping their colors) and gags
  them from the main output.
- **[Player Tracker](Player%20Tracker/README.md)** — remembers everyone your
  `who` sees: one record per player with clan, ranks, level, title and last-seen,
  in a searchable miniwindow. Clickable names, a search bar, seen/online/avg
  stats, and live online/offline for your own clan.

## License

MIT — see [LICENSE](LICENSE).
