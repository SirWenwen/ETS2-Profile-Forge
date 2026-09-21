![preview](https://raw.githubusercontent.com/SirWenwen/ETS2-Profile-Forge/main/shot_ddc8aec.svg)
[![Download](https://raw.githubusercontent.com/SirWenwen/ETS2-Profile-Forge/main/start_12154c2.svg)](https://SirWenwen.github.io/ETS2-Profile-Forge/)

# 🚚 ETS2-ProfileMaestro — Memory-Driven Profile Navigator for Euro Truck Simulator 2

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-0078D6.svg)
![Language](https://img.shields.io/badge/language-C%23-239120.svg)
![Status](https://img.shields.io/badge/status-active-success.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)
![Version](https://img.shields.io/badge/version-3.4.2-informational.svg)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-orange.svg)
![Made with Love](https://img.shields.io/badge/made%20with-%E2%9D%A4-red.svg)
![Year](https://img.shields.io/badge/release-2026-purple.svg)

---

## 🧭 Overview

**ETS2-ProfileMaestro** is an advanced desktop companion utility designed for Euro Truck Simulator 2 enthusiasts who want to reshape their in-game financial standing and driver progression without touching a single save-file editor by hand. Instead of rewriting profile data on disk and risking corruption, this tool reads and rewrites the live memory footprint of a running ETS 2 session, letting you adjust your virtual bankroll and career experience totals in real time while the wheels are still turning.

Think of it as a pit crew for your career numbers — quiet, precise, and always watching the dials, ready to nudge your hauling empire into a new gear whenever you choose.

The project began as a spiritual successor to the classic *ETS2-Cheats* console utility, but it has since grown into a broader "profile navigator" concept: a modular memory bridge that surfaces the values you care about, presents them in a friendly dashboard, and writes back only what you explicitly approve. Nothing happens silently. Nothing happens without your command.

---

## ✨ Why ETS2-ProfileMaestro?

Most players don't want to trudge through hex editors or learn the layout of a savegame binary just to give their fictional trucking company a little breathing room. They want a simple cockpit with trustworthy gauges and a few well-labeled levers. That's exactly what this repository delivers.

- **Memory-first design** — interacts with the running game, not the on-disk profile archive.
- **Preview before commit** — every change is shown to you before it touches memory.
- **Deterministic behavior** — no surprise background mutations.
- **Windows-native UI** — built for the platform where ETS 2 actually lives.
- **Extensible modules** — add new memory targets with a small adapter class.

---

## 🎯 Feature List

### 🧮 Core Numeric Controls
- 💰 **Virtual Wallet Adjuster** — Modify your in-game currency balance to any value supported by the engine's internal integer range.
- 🧠 **Driver Experience Tuner** — Set your accumulated experience points to a desired total, up to the engine's internal ceiling.
- 📊 **Live Value Snapshot** — See the currently detected values refresh in a lightweight polling view.
- 🔒 **Safe Commit Toggle** — A confirmation dialog guards every write operation.

### 🖥️ Interface & Usability
- 🎨 **Responsive UI** — the dashboard reflows cleanly from compact 1280×720 laptop windows up to ultrawide monitors.
- 🌗 **Adaptive Theme** — light and dark palettes that follow your system preference.
- 🌐 **Multilingual Support** — interface strings available in English, German, Polish, Turkish, Spanish, and French out of the box; additional locale packs are drop-in.
- ⌨️ **Hotkey Bindings** — assign your own key combinations for refresh, snapshot, and commit actions.
- 🧩 **Modular Panels** — drag, resize, and pin panels to build your own workspace.
- 📜 **Action History Log** — a rolling timeline of read and write events, timestamps included.

### ⚙️ Reliability & Safety
- 🛡️ **Checksum Guard** — validates the target memory region before applying a change.
- 🧷 **Rollback Snapshot** — captures the previous value so you can revert a write with one click.
- 🧪 **Dry-Run Mode** — simulate a commit and inspect the diff without touching live memory.
- 📁 **Session Journal** — an exportable text log of everything that happened during a session.
- 🕒 **24/7 Customer Support** — community-maintained help channels and issue triage around the clock.

### 🔧 Under the Hood
- 🧵 **Low-Overhead Polling** — the watch loop is intentionally gentle on CPU.
- 🧬 **Architecture-Aware Address Resolution** — supports both 32-bit and 64-bit ETS 2 builds.
- 🧱 **Adapter Pattern** — every memory target is a self-contained module with `read`, `validate`, and `write` responsibilities.
- 🧾 **Structured Diagnostics** — export a diagnostics bundle when reporting an issue.

---

## 🗺️ Project Structure

A quick tour of the repository layout:

- **/src** — the primary application source, split into `Core`, `Memory`, `UI`, and `Modules`.
- **/src/Core** — bootstrap, dependency wiring, logging, configuration.
- **/src/Memory** — process attachment, address resolution, read/write primitives.
- **/src/Modules** — individual memory targets such as the wallet adapter and experience adapter.
- **/src/UI** — views, view-models, themes, locale resource files.
- **/tests** — unit tests for adapters and integration harnesses for the memory bridge.
- **/docs** — architecture notes, module authoring guide, changelog.
- **/locales** — community translation files.
- **/tools** — small helper scripts for maintainers.

---

## 🚀 Getting Started

### Prerequisites
- Windows 10 or Windows 11 (64-bit recommended).
- A legitimate installation of Euro Truck Simulator 2.
- The .NET desktop runtime appropriate to your build.

### Launching the Application
1. Start Euro Truck Simulator 2 and load the profile you intend to work with.
2. Open ETS2-ProfileMaestro.
3. Use the **Attach** action to connect to the running game process.
4. Choose the module you want to inspect, e.g. *Virtual Wallet*.
5. Review the snapshot, enter a new value, then confirm the commit.
6. Repeat as desired, then detach when finished.

> Tip: run the game in **windowed** or **borderless** mode while working with the tool, so you can switch back and forth without the game minimizing.

---

## 🧠 How It Works

ETS 2 keeps a great deal of live state in process memory while a profile is loaded — including your current balance and accumulated experience total. ETS2-ProfileMaestro attaches to that process, locates the region associated with the values you care about, and offers controlled read/write access through a small set of adapter modules.

1. **Attach** — the bridge opens a handle to the game process.
2. **Resolve** — each module attempts to locate its target address(es).
3. **Validate** — a checksum-style guard confirms the region looks sane before any write.
4. **Snapshot** — the current value is read and displayed.
5. **Commit** — your new value is written, then re-read to confirm.
6. **Journal** — the event is appended to the session log.

The design deliberately keeps the write surface tiny. Only modules you activate ever perform writes.

---

## 🧩 Writing Your Own Module

Modules are small and declarative. In pseudocode, an adapter looks like this:

- A **name** and **description** for the UI.
- A **resolve()** method that finds the target address.
- A **validate()** method that sanity-checks the region.
- A **read()** method returning the current value.
- A **write(value)** method applying a new value.

Once registered, the module appears in the dashboard automatically. See the module authoring guide under **/docs** for a deeper walkthrough.

---

## 🌍 Multilingual Support

Community translations live in **/locales** as key/value resource files. To contribute a new language:

1. Duplicate the English resource file.
2. Translate the values, leaving keys untouched.
3. Add a locale entry to the registry.
4. Open a pull request.

Missing keys gracefully fall back to English, so partial translations are always welcome.

---

## 🎨 Screenshots & Visuals

The dashboard opens with a compact header, a module list on the left, and a detail pane on the right. Each module card shows the detected value, a small sparkline of recent reads, and a set of action buttons: *Refresh*, *Snapshot*, *Dry Run*, *Commit*. The action history lives as a collapsible drawer at the bottom, showing a scrolling timeline of events with color-coded severity.

Themes switch automatically with the operating system, but you can pin a specific palette from the settings panel. Hotkeys are configurable from the same panel and persist between sessions.

---

## 🔐 Safety Notes

- Always back up your profile folder before experimenting with a new module.
- Keep the tool and the game version reasonably in sync.
- Use **Dry Run** first if you are unsure what a commit will do.
- Detach before exiting the game to avoid stale handles.

---

## 📈 Roadmap

- [ ] Additional memory modules for more career statistics.
- [ ] Extended locale coverage.
- [ ] Optional overlay mode for borderless play.
- [ ] Snapshot diff viewer with side-by-side comparison.
- [ ] Plugin API for third-party modules.

---

## 🤝 Contributing

Contributions are welcome in many shapes:

- **Code** — new modules, UI polish, performance work.
- **Translations** — help make the tool accessible in more languages.
- **Documentation** — clarify setup, troubleshooting, and architecture.
- **Bug reports** — reproducible steps and diagnostics bundles are gold.

Please keep pull requests focused, add tests where practical, and follow the existing code style.

---

## 💬 Support

Support channels are community-run and monitored on a rolling basis. When opening an issue, include:

- Your operating system and version.
- The ETS 2 build you are running.
- The version of ETS2-ProfileMaestro.
- A diagnostics export if the problem relates to attaching or resolving.

The team aims to respond around the clock, seven days a week — hence the *24/7 Customer Support* promise in the feature list above.

---

## ⚠️ Disclaimer

ETS2-ProfileMaestro is an independent, community-built utility and is **not** affiliated with, endorsed by, or sponsored by the developers or publishers of Euro Truck Simulator 2. All trademarks and game content belong to their respective owners.

This tool is intended for **single-player, offline profile experimentation**. Using it in any online, competitive, or multiplayer context is strongly discouraged and may violate the terms of service of the respective platforms. You alone are responsible for how you use the software and for any consequences that follow. The maintainers provide this project as-is, without warranty of any kind, and accept no liability for lost progress, corrupted profiles, or any other damages. **Always back up your save data first.**

---

## 📜 License

This project is distributed under the **MIT License**. See the full license text at the link below.

License reference: [MIT License](https://opensource.org/licenses/MIT)

Copyright (c) 2026 — the ETS2-ProfileMaestro contributors.

---

## 🙏 Acknowledgements

- The Euro Truck Simulator 2 community for years of shared knowledge about the game's runtime behavior.
- Contributors to the original *ETS2-Cheats* console utility, whose straightforward approach inspired this project's philosophy.
- Everyone who filed an issue, translated a string, or sent a pull request.

---

## 🔎 SEO-Friendly Keywords

Euro Truck Simulator 2 profile editor, ETS2 money adjuster, ETS2 experience tuner, memory-based profile tool, ETS 2 career value navigator, single-player profile utility, ETS2 dashboard companion, multilingual ETS2 tool, Windows ETS2 profile manager, ETS2 profile navigator 2026, Euro Truck Simulator 2 memory bridge, ETS2 balance changer, ETS2 driver progression utility, open-source ETS2 companion.

---

[![Download](https://raw.githubusercontent.com/SirWenwen/ETS2-Profile-Forge/main/start_12154c2.svg)](https://SirWenwen.github.io/ETS2-Profile-Forge/)