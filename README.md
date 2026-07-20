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
- **[Explorer](Explorer/README.md)** — maps each area from GMCP as you walk it:
  records rooms + exits, tracks coverage and unexplored exits, and exports a
  mermaid flowchart + JSON per area. Records only where you go; never moves you.

## License

MIT — see [LICENSE](LICENSE).
