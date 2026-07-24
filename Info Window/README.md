# Info Window

Captures every `INFO: …` line into its own miniwindow — keeping the original
colors — and gags it (and the blank line Aardwolf pads it with) from the main
output.

## Requires

- the Aardwolf client package (uses `movewindow`, the z-order monitor, and the
  `end_gag` blank-gag trick from `aard_chat_echo`)

## Use

- **`infowin`** — show/hide the window.
- **Drag** the title bar to move (position remembered).
- **Drag the bottom-right grip** to resize.
- **Mouse-wheel** to scroll back through the buffer.
- **Right-click** the title bar → Bring to Front / Send to Back / Hide / Clear.

| command | what |
|---|---|
| `infowin` | show/hide the window |
| `infowin buffer <n>` | scrollback lines to keep (10–5000, default 100) |
| `infowin buffer` | show the current setting and how many are held |
| `infowin clear` | empty the buffer |
| `infowin show` / `infowin hide` | force it open/closed |

Size, position and buffer length persist across sessions.

## Notes

- The `^INFO: (.*)$` trigger uses `omit_from_output` to gag the line, and enables
  an `end_gag` group to gag the trailing blank Aardwolf adds — the same technique
  the package's comm window uses.
- If a separate chat/comm plugin *also* shows INFO, that's that plugin's capture;
  turn INFO off there or tell it not to grab it.
