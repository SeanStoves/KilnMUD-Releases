# Changelog

All notable changes to KilnMUD are documented here.

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
