# BGM Forge

**BGM Forge** is a freeware Windows desktop application for editing music and sound configurations in M.U.G.E.N game engines. It scans stages, manages a sound bank, edits system.def, and configures Mugen-Hook addon files — all from a multi-tabbed GUI with full language support (ES/EN/PT/RU).

## Features

- **Stage Music Editor** — scan `stages/`, edit bgmusic, volume, loop points for one or multiple stages at once. Smart rewriting only modifies what changed. Creates `[Music]` block if missing.
- **select.def Cross-Reference** — auto-detects which stages are enabled/disabled in your roster.
- **Common1 Detection** — auto-scans `data/mkp/common1.cns` for programmed stage transitions (marked in red).
- **System Music Editor** — edit system.def slots (title, select, vs, victory): BGM path, volume, loop, plus sound effect parameters (cursor.move.snd, etc.) per section.
- **Mugen-Hook Configurator** — edit mugenhook.ini settings with boolean dropdowns, plus cfg/soundAnn.dat character announcer data (supports both operation modes 0 and 1).
- **Sound Bank** — persistent audio library with Play/Stop preview (requires pygame). Color-coded: blue = in use by a stage, gray = file missing, green/yellow = currently playing.
- **4 Languages** — Español, English, Português, Русский. Switch anytime, persists across sessions.
- **Column Sorting** — click any column header to sort (stages, bank, soundAnn tables).
- **Centralized Backups** — `.def.bak` stored in `backups/<game_id>/`, not in your M.U.G.E.N folder.
- **Session Persistence** — remembers your M.U.G.E.N root, filters, bank, and UI state.

![](https://raw.githubusercontent.com/deenbee/bgm_forge/main/screenshot/shot1.png)  

![](https://raw.githubusercontent.com/deenbee/bgm_forge/main/screenshot/shot7.png)  

![](https://raw.githubusercontent.com/deenbee/bgm_forge/main/screenshot/shot8.png)  

![](https://raw.githubusercontent.com/deenbee/bgm_forge/main/screenshot/shot3.png)  

## Requirements

- Windows (tested on 10 / 11)
- Optional: [pygame](https://www.pygame.org/) for Play/Stop audio preview

## Quick Start

1. Launch BGM Forge.
2. Click **Browse** next to "M.U.G.E.N root" and select the folder containing `mugen.exe`.
3. Click **Auto** to detect stages and optional config files.
4. Use the tabs to navigate:
   - **Stage List** — select stages and apply new audio/volume/loop settings.
   - **System Music** — edit system.def music slots and sound effects.
   - **Sound Bank** — import audio files and preview them.
   - **Mugen-Hook** — configure mugenhook.ini and soundAnn.dat.

## Backups

Backups are created automatically before any modification and stored in `backups/<game_id>/` inside the app folder. They never touch your M.U.G.E.N installation. Use the **Migrate** button to move old-style backups (`.def.bak` next to your stages) into the new system.

> Audio files are never copied. Only the path reference is written to the `.def`.

## Notes

- Tested with M.U.G.E.N 1.1 beta and IKEMEN.
- The recommended location for audio files is the `sound/` folder inside your M.U.G.E.N root.
- If a file is not found on disk, the bank entry is shown in gray but kept in the list.

## License

Freeware — All rights reserved.
(c) 2026 dnbstartrek
