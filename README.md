# LoL Draft Assistant — for lolmanager.com

> ⚠️ **This project is designed exclusively for [lolmanager.com](https://lolmanager.com), the LoL manager game by Night.**
> The counterpick, synergy and matchup data embedded in the app comes directly from that game and is **not** representative of the real League of Legends meta or any other game. This tool has no use outside of lolmanager.com.

Desktop tool to optimize your drafts in Night's LoL manager. LoL client-style interface with 170 champions, 2464 counterpicks and 692 synergies specific to the game.

## Overview

The tool helps you:
- Evaluate lane matchups and team synergy in real time based on lolmanager.com data
- Recommend the best picks per role according to the enemy composition and your locked allies
- Suggest the most threatening bans
- Automatically compute the optimal draft using a Simulated Annealing algorithm

## Features

- **170 champions** with their roles according to lolmanager.com
- **2464 asymmetric counterpicks** (A counters B ≠ B counters A)
- **692 synergies** between allies
- **Undo/Redo** (Ctrl+Z / Ctrl+Y) with 50-action history
- **Smart filtering**: banned or already-picked champions disappear from recommendations
- **Real-time updates** of the optimal draft on every pick/ban
- **Role picker** for flex champions (Top/Jungle, Mid/Bot, etc.)
- **Clear draft phases**: visually distinct buttons for ban ally, pick ally, pick enemy, ban enemy
- **Official portraits** fetched from Data Dragon (Riot's CDN) for display

## ⚠️ Important — Project scope

The data embedded in this script (counterpicks, synergies, roles) comes **exclusively from Night's lolmanager.com**. It does **not** correspond to:
- Real League of Legends matchups on Summoner's Rift
- External tier lists or guides (u.gg, op.gg, lolalytics, etc.)
- Any other meta or version of the game

Using this tool to draft in the real LoL therefore makes no sense. It is designed and calibrated only to optimize drafts inside the manager game.

## Installation

### Option 1: Windows executable (recommended)

Download `LoLDraftAssistant.exe` from the [Releases page](../../releases) and run it. No Python installation needed.

### Option 2: Run the Python script

Requirements: Python 3.8+ with Tkinter (included by default on Windows).

```bash
pip install Pillow
python lol_draft.py
```

On first launch, the app downloads champion portraits (~3 MB) to `~/.lol_draft_cache/`. Subsequent launches use the local cache.

### Option 3: Build the executable yourself

```bash
pip install pyinstaller pillow
pyinstaller --onefile --windowed --name "LoLDraftAssistant" lol_draft.py
```

The executable will be in `dist/LoLDraftAssistant.exe`.

## Usage

1. **Select a phase** at the top right: 🛡 BAN ALLY, ✓ PICK ALLY, ⚔ PICK ENEMY, 🛡 BAN ENEMY
2. **Click a champion** in the central grid:
   - If the champion has only one possible role, it's placed directly at the right spot
   - If the champion is flex (e.g. Sett Top/Support), a menu asks which role to assign
3. **Click a filled slot** to clear it
4. **UNDO / REDO** to undo/redo (or Ctrl+Z / Ctrl+Y)
5. **APPLY** in the footer to commit the suggested optimal draft

## How the model works

The draft score combines:
- Direct lane matchups (×1.3)
- Cross matchups (jungle/support vs other lanes, ×0.7)
- Synergies between allies (×1.8)
- Penalty for enemy synergies against your champions (×0.4)
- Weakness penalty: -4 if any lane is -3 or worse

The optimizer uses Simulated Annealing with 3000-5000 iterations to explore the millions of possible combinations while respecting your locked picks, bans and enemy champions.

## Data structure

Data (from lolmanager.com) is embedded in the script:
- `CHAMPS_ROLES`: dict `{name: [roles]}`
- `COUNTERS`: dict `{a: {b: strength}}` for "a counters b"
- `SYNERGIES`: dict `{a: {b: strength}}`, symmetric
- `DISPLAY_TO_INTERNAL` / `INTERNAL_TO_DISPLAY`: display aliases (K'Sante ↔ KSante, Wukong ↔ MonkeyKing, etc.)

## Tech stack

- Python 3 + Tkinter for the interface
- Pillow (PIL) for portrait processing
- Data Dragon (Riot Games) for champion portraits
- PyInstaller for .exe compilation

## License

Personal project, not affiliated with Night or lolmanager.com. League of Legends and its assets belong to Riot Games. lolmanager.com is an independent game by Night.
