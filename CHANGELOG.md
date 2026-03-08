# Changelog

All notable changes to KilnMUD are documented here.

## [v1.5.5] — Fix Profile+Plugin Connect Crash (2026-03-08)

## Bug Fix

Fixes a crash when connecting via a saved profile that has plugins loaded.

### Root Cause
`CreateWildcardsTable` in the Lua engine used a temporary global variable (`__tmp_wildcards`) to create wildcard tables for trigger/alias callbacks. After clearing the global reference, the Lua GC could collect the table before it was used by the script callback, causing a native access violation that crashed the app.

### Fix
- Replaced temporary global approach with anonymous table creation (`return {}`) that is properly referenced by the C# `LuaTable` wrapper
- Added defensive error handling in `PluginScriptHost.CallFunction` for wildcard conversion failures
- Protected `WorldConnection.onConnected` and line processing handlers from unhandled exceptions on background threads

## [v1.5.4] — Native .kiln Format & UI Enhancements (2026-03-08)

## What's New

### Native .kiln World File Format
- New JSON-based `.kiln` format as the default save format
- Human-readable, clean DTO structure — no legacy XML quirks
- Open/Save/Save As dialogs default to `.kiln`, with `.mcl`/`.xml` still supported

### Themed Input Line
- Input box text and background now follow the active theme
- No more invisible text on dark themes — colors properly applied from `InputForeground`, `InputBackground`, `InputBorder`

### Output Context Menu
- Right-click on output text for context menu
- Copy, Select All
- **Create Trigger from Selection** — opens trigger editor pre-populated with the clicked line
- **Create Alias from Selection** — opens alias editor pre-populated with the clicked line

### Plugin Fixes (from v1.5.3)
- Fix `GetInfo(72)` returning nil (CowBar compatibility)
- Fix wildcard tables not passed to trigger/alias script callbacks (QuowMindspace compatibility)
- Regex captures now correctly passed as 1-based Lua tables to MUSHclient-style callbacks

## [v1.5.3] — Fix Plugin Script Initialization (2026-03-08)

## Bug Fixes

- **CowBar `string.sub` nil error** — `GetPluginID()` / `GetPluginName()` now return the plugin's own identity in plugin context instead of the world's
- **QuowMindspace `json` is nil** — JSON library is now registered as a global, not just via `require()`
- **QuowMissions concatenate nil** — Added `GetPluginVariable` / `SetPluginVariable` for inter-plugin variable sharing
- **About dialog** — Logo now displays alongside the title with improved layout

## [v1.5.2] — About Dialog & Public Release Workflow (2026-03-08)

### Improvements

- **About dialog** — Help > About KilnMUD now opens a styled dialog with version, author credit, and a clickable link to report bugs or view the changelog
- **Public release workflow** — CI now automatically publishes releases and build artifacts to the public repo

## [v1.5.1] — MUSHclient Plugin API Compatibility (2026-03-08)

### Fixes & Improvements

- **CallPlugin** — Inter-plugin communication (145 CowBar calls)
- **ColourNameToRGB / RGBColourToName** — Color name ↔ RGB conversion (197 calls)
- **Hyperlink** — Clickable links in MUD output (161 calls)
- **miniwin.\* constants** — Full MUSHclient miniwindow constants table (rect ops, hotspot flags, cursors, positioning)
- **bit library shim** — Lua 5.1 LuaBitOp compatibility for Lua 5.4 (`bit.band`, `bit.bor`, `bit.bxor`, etc.)
- **lsqlite3 shim** — SQLite from Lua via `sqlite3.open()`, `db:nrows()`, `db:exec()` wrapping WorldApi Database functions
- **utils.\* dialogs** — `utils.inputbox`, `utils.listbox`, `utils.msgbox`, `utils.filepicker`, `utils.directorypicker`
- **GetInfo 272-281** — Screen/text area dimensions for miniwindow layout
- **Redraw/Repaint** — Wired to UI invalidation
- **Clipboard** — Wired to Avalonia clipboard API
- **XML sanitizer fix** — No longer corrupts `<=` inside `<script>` elements
- **Plugin loading fix** — Prevent duplicate loading on reconnect
- **Native plugin format** — `.plugin.json` + separate script files via PluginExporter
- **Profile bundles** — Directory-based profiles with plugin import from MUSHclient XML

## [v1.5.0] — Profiles & Plugin Scripting Fixes (2026-03-08)

### Connection Profiles

- Create, save, edit, and delete named connection profiles
- Store server, credentials, auto-connect text, world file, script language, and notes
- Accessible from Connection > Profiles menu and the welcome screen
- Profiles auto-save on connect and persist across sessions

### Plugin Scripting Fixes

- Full MUSHclient API now available to plugin scripts (76+ functions)
- Shared Lua globals (rex, json, utils) for both world and plugin scripts
- Added `GetPluginInfo()` for querying plugin metadata by ID
- Fixed plugins failing with missing API globals

## [v1.4.1] — Fix Plugin Loading and Installer Versioning (2026-03-08)

- **Plugin loading**: MUSHclient plugin files with PCRE regex patterns now load correctly
- **World file loading**: Same XML sanitization applied to `.mcl` world file loading
- **PCRE-to-.NET conversion**: Plugin trigger and alias regex patterns automatically converted
- **Installer version**: Fixed Inno Setup installer always showing version 1.3.0

## [v1.4.0] — Plugin Manager, Theme System, UI Overhaul (2026-03-08)

### Plugin Manager

- New **Game > Plugins** dialog to add, remove, enable/disable, and reload MUSHclient `.xml` plugin files
- Plugin paths saved with world for automatic loading on reconnect

### Theme System

- 5 built-in themes: Default Dark, Default Light, Monokai, Solarized Dark, Nord
- Create unlimited custom themes via Save As in Preferences > Appearance
- Import/Export themes as `.json` files to share with others
- Live color swatch editors for all UI chrome colors

### Preferences Overhaul

- Redesigned with left-side tab navigation
- New Appearance tab for full theme customization
- Apply button to preview changes without closing

### UI Improvements

- Dark theme by default
- Welcome screen on startup
- Closing the last tab returns to the welcome screen

## [v1.3.2] — Fix Startup Crash, Version Tagging, Signed Installer (2026-03-07)

- Fixed startup crash — icon.png included as AvaloniaResource
- Exe version now correctly matches release tag
- Signed installer — setup exe signed with self-signed cert

## [v1.3.1] — Bugfix: Startup Crash on Windows (2026-03-07)

- Fixed startup crash — Window icon path used filesystem path instead of Avalonia resource URI
- Added crash logging — Unhandled exceptions now write to `%LocalAppData%\KilnMUD\crash.log`

## [v1.3.0] — App Icon, Windows Installer, Code Signing (2026-03-07)

### App Icon

- Custom kiln-with-flame icon (SVG source, ICO/PNG at all sizes)

### Windows Installer (Inno Setup)

- `KilnMUD-Setup-x64.exe` — traditional Windows installer
- Auto-imports self-signed certificate
- Installs to Program Files with Start Menu shortcuts
- Associates `.mcl` world files with KilnMUD
- Clean uninstall removes cert and file associations

### Code Signing (CI)

- macOS: codesign + notarytool (gated on secrets)
- Windows: signtool with PFX certificate (gated on secrets)

## [v1.2.0] — Miniwindow API, Sound/MSP, SQLite Database (2026-03-07)

### Miniwindow Drawing API

- Full `Window*` API (24 functions): create, show, delete, position, resize
- Drawing primitives: rectangles, circles, lines, text, gradients, images, pixel ops
- Interactive hotspots with mouse callbacks, drag handlers, scroll wheel
- Font management and text measurement

### Sound & MSP

- Cross-platform `SoundPlayer` via native audio processes
- MUD Sound Protocol (MSP) processor: parses `!!SOUND()`/`!!MUSIC()` inline triggers
- Loop support, volume control, stop-all

### SQLite Database API

- Full `Database*` API (15 functions) powered by Microsoft.Data.Sqlite

### Lua Modules

- Bundled utility modules: gauge, mapper, movewindow, serialize, tprint, wait, colors, json

## [v1.1.0] — Scripting & Import Enhancements (2026-03-07)

- Lua, JavaScript, and Python scripting engines
- MUSHclient world API (100+ functions)
- Mudlet package import

## [v1.0.0] — Initial Release (2026-03-07)

- Tabbed multi-world sessions
- MUSHclient world file (.mcl) import
- ANSI 16/256/24-bit color
- Telnet (MCCP, NAWS, TTYPE/MTTS, CHARSET)
- Triggers, aliases, timers with regex support
- Command history, tab completion, command stacking
- Speedwalk, auto-say
- Cross-platform: Windows, macOS, Linux

