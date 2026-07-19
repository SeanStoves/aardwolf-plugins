# aardwolf-plugins

MUSHclient plugins for the [Aardwolf](https://www.aardwolf.com/) MUD.

These run inside the [Aardwolf MUSHclient client package](https://github.com/fiendish/aardwolfclientpackage)
— they lean on its bundled Lua libs (`movewindow`, `serialize`, `constants.lua`)
and the standard GMCP handler. Drop a plugin's `.xml` anywhere in your plugins
directory; nothing depends on the folder it lives in.

## Install

1. MUSHclient → **File → Plugins → Add…**
2. Pick the plugin's `.xml`.
3. Done. Type the plugin's help command (below) to configure it.

To keep it loaded across restarts, either leave it added (MUSHclient remembers)
or add an `<include name="RecallManager.xml" plugin="y" />` line to your world
file's plugins block.

## Plugins

### Recall Manager (`RecallManager.xml`)

A miniwindow of **clickable buttons you build yourself** — each fires a list of
commands. Good for recalls, portals, held-item transports, shop runs, anything
you do the same way every time. Ships empty; nothing is assumed about your
character. Optional per-button arrival detection updates an "At:" readout.

The miniwindow shows `Recall Manager v<version>` on its title bar.

Commands (type `recallmanager help`):

```
recallmanager add <name> <cmd|cmd|...>    add a button
recallmanager edit <name> <cmd|cmd|...>   change a button's commands
recallmanager del <name>                  remove a button
recallmanager at <name> <regexp|off>      when that line is seen, set 'At:' to <name>
recallmanager <name>                      fire a button (same as clicking it)
recallmanager list                        show your buttons
recallmanager reset                       zero the counters
recallmanager                             show/hide the window
recallmanager help                        this
```

**Commands are separated by `|`, not `;`.** Aardwolf's command stacking splits a
typed `;` before the plugin ever sees it, so the pipe is the safe delimiter.

Example — a magic-bus transport (held item, by object id), a plain recall, and a
portal with an arrival line:

```
recallmanager add bus get 12345 67890|hold 12345|enter|dual 55555|put 12345 67890
recallmanager at  bus ^WHOOOOOOOOOOOOSH!$
recallmanager add recall recall
recallmanager add portal get 11111 67890|hold 11111|enter|put 11111 67890
recallmanager at  portal ^Inside the Academy Foyer
```

Get an item's object id in-game with the `id` spell/command. Drag the window by
its title bar; right-click it for Hide / Reset. All buttons, counters, and window
position persist across sessions.

## License

MIT — see [LICENSE](LICENSE).
