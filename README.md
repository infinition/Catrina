# Catrina

<img width="250" height="250" alt="catrina - Copie" src="https://github.com/user-attachments/assets/ae992331-b786-4af0-8009-fd7d9df9a6cb" />

![Python](https://img.shields.io/badge/Python-3776AB?logo=python\&logoColor=fff)
![Platform](https://img.shields.io/badge/Platform-Windows-blue)
![Status](https://img.shields.io/badge/Status-Development-blue.svg)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Catrina is a Windows desktop app (PyQt6) for building and running keyboard/mouse **macros** and a **gaming remap (hold)** mode. It focuses on reproducible desktop automation: typed text, special keys, timed waits, mouse moves/clicks/scroll, and drag sequences with absolute coordinates. A simple token language powers the executor; a **Recorder** can capture real workflows in seconds.


<img width="789" height="972" alt="image" src="https://github.com/user-attachments/assets/37c2c9a3-0449-4450-8c1e-01b9b002d4c6" />

---

## Table of Contents

* [What Catrina Does](#what-catrina-does)
* [Feature Overview](#feature-overview)
* [How It Works](#how-it-works)
* [Quick Start](#quick-start)

  * [A Visible Macro (no recording)](#a-visible-macro-no-recording)
  * [Record a Workflow (recommended)](#record-a-workflow-recommended)
  * [Gaming Remap (hold)](#gaming-remap-hold)
* [Tokens Cheat-Sheet (excerpt)](#tokens-cheat-sheet-excerpt)
* [Install & Run](#install--run)

  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
  * [Run](#run)
* [Tips & Limitations](#tips--limitations)
* [Troubleshooting](#troubleshooting)
* [Contributing](#contributing)
* [License](#license)

---

## What Catrina Does

* Executes **repeatable desktop workflows** on Windows using a compact, readable token syntax.
* Captures **real user interactions** (keyboard, mouse, scroll, drag with coordinates) via **Record** and converts them to tokens—one action per line.
* Provides a **Gaming Remap (hold)** mode: map a trigger key to other keys while held (press on down, release on up).
* Shows the current action in a status bar and optional **always-on-top overlay**.

This is not a headless RPA engine; it automates the active desktop session and does exactly what you record or write.

---

## Feature Overview

* **Tabs-only UI**: each macro is a tab (no card view).
* **Record mode** (F8/F12 to stop): captures keyboard, mouse clicks, scrolls, and drags (start/end + positions).
* **Text tab**: token editor with **one action per line** (letters included) and color highlighting.
* **Steps tab**: visual list of parsed steps; drag to reorder; context menu to replace a step.
* **Delays jitter**: if **Min/Max** delays are set in the macro, each `[WAIT{…}]` is automatically increased by a random amount between Min and Max.
* **Gaming Remap (hold)** (F9): while holding the trigger key, mapped keys are pressed; on release, they’re released. No text/mouse/waits run in this mode.
* **Xbox virtual gamepad (optional)**: via `vgamepad` (disabled by default; toggle via icon/checkbox).
* **Overlay**: optional, always-on-top panel showing the current action; configurable corner and opacity.
* **i18n**: simple language toggle (EN / FR / IT / ES / DE). UI strings are translated; key names are not.
* **Suggestions**: dropdowns and completers for common keys/combos.
* **Autosave**: automatically saves to `catrina.catrina` (JSON). If present in the working folder, it’s auto-loaded. A legacy `catrina.json` is also written for compatibility.
* **Logging & Debug**: rotating `catrina.log`, optional debug level.
* **Global hotkeys**: F10 Start, F11 Stop, F9 Gaming Remap toggle, F8/F12 stop recording.

---

## How It Works

Catrina parses a **token sequence** and executes each action in order. The **Recorder** emits the same tokens you would type by hand, including waits and mouse coordinates. The executor uses Windows low-level input APIs where possible (SendInput) and a key map for many special keys (F13–F24, multimedia, browser, numpad, L/R modifiers, etc.).

* **One action per line** in the **Text** tab (even for plain letters).
* The **Steps** tab shows parsed items with categories and colors.

Examples of tokens are in the [Tokens Cheat-Sheet](#tokens-cheat-sheet-excerpt).

---

## Quick Start

### A Visible Macro (no recording)

This macro opens **Notepad**, types a message, and saves a file. It demonstrates waits, text, and shortcuts.

1. Run Catrina (see [Install & Run](#install--run)).
2. Click **＋ Macro**, name it e.g. `Notepad Demo`.
3. Set **Trigger** to: `ctrl+alt+n`
4. Open **Text** and paste the sequence (one action per line):

```
[WIN+R]
[WAIT{300}]
notepad
[ENTER]
[WAIT{700}]
Catrina demo — opened, typed and saved by a macro.
[WAIT{200}]
[CTRL+S]
[WAIT{400}]
demo.txt
[ENTER]
```

5. Click **Start (F10)**. Press **CTRL+ALT+N**.

You should see Run → Notepad → text typed → Save dialog → `demo.txt` saved.

> Notes
>
> * Keep **small non-zero waits**, UI needs time to react.
> * If you set **Min/Max** delays in the macro, each `[WAIT{…}]` will get a random extra delay within that range.
> * Enable **Overlay** if you want to see the current action on screen.

### Record a Workflow (recommended)

Recording is the fastest way to build real, multi-step sequences.

1. In a macro tab, click **Record**.
2. Perform the steps: type, click, scroll, **drag** (window elements, sliders, etc.).

   * Clicks are emitted with a preceding `MOUSE_MOVE^x*y`.
   * Drags become `MOUSE_DRAGSTART_*` → `MOUSE_DRAGEND_*` with positions.
   * Timing is captured as `[WAIT{ms}]`.
3. Press **F8** or **F12** to stop.
4. The **Text** tab is auto-filled, one action per line, ready to run or tweak.

If execution is too fast/slow, adjust waits or set **Min/Max** jitter.

### Gaming Remap (hold)

This mode maps a **trigger** to **keys** that are held while the trigger is pressed.

* In the macro tab, check **Gaming remap (hold)**.
* Set **Trigger** (e.g. `caps lock`) and **Keys** (e.g. `CTRL+SHIFT`).
* Toggle mode with **F9**. While you **hold** Caps Lock, Catrina presses **CTRL**+**SHIFT**; on release, it releases them.

> In Gaming mode: **only** hold-style key mapping runs (no text/mouse/waits).
> Xbox virtual gamepad support is available via `vgamepad` (off by default).

---

## Tokens Cheat-Sheet (excerpt)

* **Keys**: `[ENTER]`, `[ESC]`, `[ALT+F4]`, `[CTRL+S]`, `[WIN+R]`, `[F5]`, `[NUMPAD5]`, etc.
* **Wait**: `[WAIT{300}]` (milliseconds) or `[WAIT{1.5s}]`.
* **Text**: plain characters on their own line (e.g. `hello world`).
* **Mouse**:

  * Move: `[MOUSE_MOVE^960*540]`
  * Click: `[MOUSE_CLICKLEFT]`, `[MOUSE_CLICKRIGHT]`, `[MOUSE_CLICKMIDDLE]`
  * Scroll: `[MOUSE_SCROLLUP]`, `[MOUSE_SCROLLDOWN]`
  * Drag: `[MOUSE_DRAGSTART_LEFT]` … `[MOUSE_DRAGEND_LEFT]`
* **Hold / Long press**:

  * Hold combo with duration: `[CTRL+SHIFT{120}]`
  * Long press single key: `[LONGPRESS[SPACE]{500}]`
* **Gamepad (optional)**:

  * Buttons: `[GP_BTN[A]]`, `[GP_HOLD[RB]{200}]`
  * Triggers: `[GP_TRIGL{180}]`, `[GP_TRIGR{200}]`
  * Sticks: `[GP_STICKL^40*-80{120}]`

> System security: **CTRL+ALT+DELETE** (SAS) cannot be emitted.

---

## Install & Run

### Prerequisites

* **Windows** (tested on recent versions).
* **Python 3.9+** recommended.
* It’s often best to run the app with **Administrator** privileges so global hooks can register cleanly.

### Installation

```bash
# Clone your repository
git clone https://github.com/<you>/catrina
cd catrina

# (Optional) Create a virtual environment
python -m venv .venv
.venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

**Typical requirements** (already covered by `requirements.txt` if present):

* `PyQt6`
* `keyboard`
* `mouse`
* `pywin32`
* `vgamepad` (optional; only if you need Xbox virtual gamepad)

### Run

```bash
python catrina.py
```

* **Start/Stop**: F10 / F11
* **Gaming Remap toggle**: F9
* **Stop Recording**: F8 or F12

Autosave file: `catrina.catrina` in the working directory (auto-imported on launch). A legacy `catrina.json` is also written.

---

## Tips & Limitations

* **Absolute coordinates**: mouse moves/clicks/drags use screen coordinates; if you change resolution, window layout, or scaling (DPI), adjust accordingly.
* **Waits**: GUI is asynchronous—small waits prevent flakiness. Use **Min/Max** to add jitter automatically to every `[WAIT{…}]`.
* **Focus**: macros interact with the foreground application; ensure the right window has focus before sending input.
* **Security**: protected sequences like **CTRL+ALT+DELETE** cannot be synthesized by design.
* **Gamepad**: Xbox virtual gamepad requires `vgamepad` and is **off by default**.

---

## Troubleshooting

* **Overlay not visible**

  * Enable the **Overlay** checkbox, choose a corner (TL/TR/BL/BR), adjust opacity.
  * If using multiple monitors, the overlay uses the primary screen’s available geometry.

* **Global hotkeys don’t register**

  * Try running as **Administrator**.
  * Make sure no other app is capturing the same hotkeys.

* **Recorded drags click instead**

  * Very short drags below the threshold may register as clicks. Drag a bit further or increase the drag threshold in code if needed.

* **Too fast / too slow**

  * Insert/adjust `[WAIT{…}]` lines or set **Min/Max** jitter in the macro.

Logs are written to `catrina.log` (rotating). Enable **Debug** in the UI to increase verbosity.

---

## Contributing

Bug reports and small PRs are welcome:

* Keep changes focused and documented.
* Include repro steps and logs where relevant.
* Avoid adding hype; document what the app actually does.
* 
---

## 🌠 Stargazers

[![Star History Chart](https://api.star-history.com/svg?repos=catrina/catrina,infinition/Catrina&type=Date)](https://www.star-history.com/#catrina/catrina&infinition/Catrina&Date)
---

## License

Catrina is distributed under the **MIT License**. See [LICENSE](LICENSE).
