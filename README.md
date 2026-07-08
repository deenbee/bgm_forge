# BGM Forge

**BGM Forge** is a Windows desktop application for editing music and sound configurations in M.U.G.E.N game engines. It scans stages, manages a sound bank, edits system.def, and configures Mugen-Hook addon files — all from a multi-tabbed GUI with full i18n support (ES/EN/PT/RU).

## Features

### Stage Music Management (Stage List tab)
- Recursive scan of `stages/` with `[Music]` block detection
- Edit 4 keys: `bgmusic`, `bgmvolume`, `bgmloopstart`, `bgmloopend`
- **Smart rewriting**: only modifies keys that differ; creates `[Music]` block if missing
- **Cross-reference with `select.def`**: auto-detects which stages are enabled/disabled
- **Multi-select**: apply changes to 1 stage, N selected, or ALL stages
- **Column sorting**: click headers to sort by Stage, BGM, Vol, Loopstart, Loopend, Select
- **Common1 detection**: auto-scans `data/mkp/common1.cns` for programmed stage transitions (marked in red with tooltip)
- **Centralized backups**: `.def.bak` stored in `backups/<game_id>/` (not in the game folder)
- **Info fields**: displays `[Info] name/author/versiondate` from system.def
- **Right-click context menu**: sort, open editor, copy path, check/uncheck selected stages

### System Music Editor (System Music tab)
- Parse and edit `data/system.def` slots: **title**, **select**, **vs**, **victory**
- Edit BGM path, volume, loop flag, loopstart, loopend
- **Sound parameters (snd)**: edit `cursor.move.snd`, `cancel.snd`, and other sound effects — organized by section (`[Title Info]`, `[Select Info]`, `[Option Info]`) with checkbox to comment/uncomment lines
- Confirmation dialog with checkboxes to apply only Bank / Volume / Loop changes

### Mugen-Hook Configurator (Mugen-Hook tab)
- **Left panel**: edit `mugenhook.ini` — all `[Settings]` and `[Strings]` parameters with descriptions
- Boolean values (`bHookCursorTable`, etc.) are shown as true/false comboboxes
- **Right panel**: view and edit `cfg/soundAnn.dat` — character select screen announcer sound data
- Supports both operation modes (0: row/col, 1: charId) auto-detected from `iCursorTableOperationType`
- Add, edit (dialog), and delete entries
- Sortable TreeView columns (Character, Row/CharID, Group, Index, P2 Group, P2 Index)
- Blocking edits: preserves comments and file structure
- Unsaved changes detection on app close

### Sound Bank (Sound Bank tab)
- Persistent sound bank stored in `bank.json`
- Auto-ID from filename (suffix `_2`, `_3` on collision)
- **Play/Stop** preview (requires `pygame`)
- **Color coding**: blue = file used by a stage, gray = file missing on disk, green/yellow = currently playing
- **Columns**: ID, Path, Included (yes/no), On disk (OK/!)
- Sortable columns (click headers)
- Bank reload and health check

### General
- **4 languages**: Español, English, Português, Русский — instant switch, persists in `settings.json`
- **Session persistence**: remembers M.U.G.E.N root, filter, bank state, UI settings
- **Confirmation dialogs**: choose which categories to apply (bank / volume / loop)
- **App icon**: custom BGM Forge icon in window and taskbar
- **About dialog**: app info, version (c) 2026, PayPal donate link, 256x256 logo

## Requirements

- Python 3.10+ (tested with 3.13)
- Tkinter (included in standard Python installation)
- Optional: `pygame` for Play/Stop preview. Install with:
  ```bash
  pip install pygame
  ```

## Usage

```bash
python main.py
```

### Quick Start

1. **Stage List tab**
   - Click *Browse* next to "M.U.G.E.N root" and select the folder containing `mugen.exe`.
   - Click *Auto* to detect `stages/` (and optional `select.def` in `data/`).
   - Configure audio path (browse a file or pick from the sound bank).
   - Select stages with Ctrl/Shift+Click, tick checkboxes, and click *Apply selected*.

2. **System Music tab**
   - Click *Auto* to find `data/system.def`.
   - Select a slot (title, select, vs, victory) and edit BGM/volume/loop parameters.
   - Edit sound effect params (snd) in the right panel — tick to activate, untick to comment.
   - Click *Apply*.

3. **Sound Bank tab**
   - Click *+ Add sound* to import audio files.
   - Select an entry and press *Play* to preview.
   - Green/Yellow highlight = currently playing; Blue = used by a stage.

4. **Mugen-Hook tab**
   - If `mugenhook.ini` exists in the M.U.G.E.N root, its settings load automatically.
   - Edit values directly (booleans use a dropdown).
   - Switch to the right panel to edit `cfg/soundAnn.dat` — add/edit/delete character announcer entries.
   - Click *Save* to persist changes.

### PyInstaller Build

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --icon=assets/app.ico --add-data "assets/app.ico;assets" --add-data "assets/icons_pngs;assets/icons_pngs" main.py
```

## Project Structure

```
bgm_forge/
├── main.py              # Tkinter UI (entry point)
├── scanner.py           # Stage scanning and .def parsing
├── scanner_select.py    # select.def parser (ExtraStages/Characters)
├── music_replacer.py    # Smart rewriting engine
├── system_def.py        # system.def parser (slots + sound params)
├── system_replacer.py   # system.def rewriting engine
├── sound_bank.py        # Sound bank with JSON persistence
├── audio_player.py      # pygame audio player (Play/Stop)
├── backup.py            # Centralized .def backup management
├── mugen_hook.py        # mugenhook.ini + soundAnn.dat parser
├── i18n.py              # Translations (es/en/pt/ru)
├── config.py            # Constants and settings
├── session.py           # Session persistence
├── assets/
│   ├── app.ico          # Application icon
│   └── icons_pngs/      # PNG icon set (16x16 to 256x256)
├── bank.json            # Bank persistence (created on save)
├── settings.json        # Language and preferences
└── README.md
```

## [Music] Block Format (M.U.G.E.N 1.1 beta)

```ini
[Music]
bgmusic = sound/your_track.ogg
bgmvolume = 100              ; 0-255
bgmloopstart = 0             ; sample position
bgmloopend = -1              ; -1 = end of file
```

## select.def Cross-Reference

The app recognizes:
- `[ExtraStages]` (M.U.G.E.N 1.1 / IKEMEN)
- `[Stages]` and `[VSStages]` (M.U.G.E.N 1.0 legacy)
- `[Characters]` with stage as second token
- Commented lines (`;`) are ignored
- Backslashes `\` normalized to `/`
- Comparison is **case-insensitive**
- Auto-detected in `data/` or subdirectories

## Backups

- Backups are stored in `backups/<game_id>/<relative_path>/<stage>.def.bak`
- One backup per `.def` (not duplicated on each apply)
- Old-style backups (`.def.bak` next to `.def`) can be migrated via the **Migrate** button

## Limitations

- Audio files are never copied — only the path is written to the `.def`
- No global undo; restore manually from `backups/`
- `pygame` required for Play/Stop preview

## License

Free for all use.
(c) 2026 dnbstartrek
