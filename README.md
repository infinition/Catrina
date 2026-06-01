# Catrina

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE) ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) ![PyQt](https://img.shields.io/badge/PyQt6-41CD52?style=flat&logo=qt&logoColor=white) ![Windows](https://img.shields.io/badge/Windows-0078D4?style=flat&logo=windows&logoColor=white) [![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=flat&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/infinition)

<img width="250" height="250" alt="Catrina" src="https://github.com/user-attachments/assets/ae992331-b786-4af0-8009-fd7d9df9a6cb" />

A Windows desktop app (PyQt6) for building and running keyboard/mouse macros and a gaming remap mode. Covers typed text, special keys, timed waits, mouse moves/clicks/scrolls, and drag sequences with absolute screen coordinates. A recorder captures real workflows live; a simple token language drives the executor.

<img width="789" height="972" alt="Catrina interface" src="https://github.com/user-attachments/assets/37c2c9a3-0449-4450-8c1e-01b9b002d4c6" />

---

## What it does

- Record keyboard and mouse actions into a replayable macro.
- Build macros manually with a token-based language.
- Gaming remap (hold) mode: remap keys with hold detection, useful for repeated or held actions in games.
- Overlay showing macro state, on any monitor corner.
- Autosave to `catrina.catrina` on exit, auto-imported on next launch.

---

## Token cheat sheet (excerpt)

| Token | Effect |
|-------|--------|
| `[TYPE{text}]` | Type a string |
| `[KEY{Enter}]` | Press a key |
| `[WAIT{500}]` | Wait 500 ms |
| `[MOVE{x,y}]` | Move mouse to absolute coordinates |
| `[CLICK{left}]` | Click mouse button |
| `[SCROLL{3}]` | Scroll wheel |
| `[DRAG{x1,y1,x2,y2}]` | Drag from point to point |

Min/Max jitter can be applied automatically to every `[WAIT{...}]` via the UI.

---

## Install and run

Requires Python 3.10+ on Windows.

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python catrina.py
```

Dependencies: `PyQt6`, `keyboard`, `mouse`, `pywin32`. `vgamepad` is optional (Xbox virtual gamepad, disabled by default).

---

## Hotkeys

| Key | Action |
|-----|--------|
| F10 | Start macro |
| F11 | Stop macro |
| F9 | Toggle gaming remap mode |
| F8 / F12 | Stop recording |

---

## Tips

- Mouse coordinates are absolute. Adjust if you change resolution or DPI.
- Add small waits between UI actions to prevent race conditions with async windows.
- Macros act on the foreground window. Make sure the right app has focus before playback.
- CTRL+ALT+DELETE cannot be synthesized by design.
- Run as Administrator if global hotkeys fail to register.

---

## Troubleshooting

- Overlay not visible: enable the Overlay checkbox and pick a corner. On multi-monitor setups it uses the primary screen geometry.
- Recorded drags become clicks: the drag distance was below the threshold. Drag further or adjust the threshold in code.
- Too fast or slow: insert `[WAIT{...}]` tokens or set Min/Max jitter in the UI.

Logs are written to `catrina.log` (rotating). Enable Debug in the UI for verbose output.

---

## Star History

<a href="https://www.star-history.com/?repos=infinition%2FCatrina&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=infinition/Catrina&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=infinition/Catrina&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=infinition/Catrina&type=date&legend=top-left" />
 </picture>
</a>

---

## License

MIT. See [LICENSE](LICENSE).
