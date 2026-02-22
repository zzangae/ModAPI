# ModAPI(v1) v2.0.9549 - 20260222

**The Forest Mod Management Tool — Upgraded Edition**

> Original: FluffyFish / Philipp Mohrenstecher (Engelskirchen, Germany)
> Upgrade: zzangae (Republic of Korea)

---

## Overview

ModAPI is a desktop application for managing mods for The Forest. This upgraded edition includes .NET Framework 4.8 migration, Windows 11 Fluent Design UI, a 3-theme system, and enhanced multilingual support.

---

## Key Changes

### Phase 1 — .NET Framework 4.8 Upgrade

- Migrated all projects (5) from `.NET Framework 4.5` → `4.8`
- Updated `TargetFrameworkVersion`, `App.config`, `packages.config` across all projects
- Unified assembly version to `v2.0.0.0`

### Phase 2 — Build Environment & Fluent Design Foundation

- Introduced **ModernWpf 0.9.6** NuGet package
- Created **FluentStyles.xaml** — Windows 11 Fluent Design override layer
  - Fluent color palette, typography, buttons, tabs, comboboxes, scrollbar styles
  - Window, SubWindow, SplashScreen templates
- Compiled **UnityEngine stub DLL**
  - Added missing types: `WWW`, `Event`, `TextEditor`, `Physics`, etc.
  - Added `TextEditor.pos`/`selectPos` properties
- Fixed dependency references and confirmed successful build

### Phase 3 — UI Redesign & Theme System

#### Fluent UI Redesign
- Complete **MainWindow.xaml** restructuring (545→565 lines)
  - Fluent Design-based layout, colors, typography
  - Redesigned tab controls, status bar, caption buttons
- Runtime fixes: SplashScreen freezing, tab switching, icon states, window dragging

#### 3-Theme System

| Theme | Style File | Description |
|-------|-----------|-------------|
| Classic | Dictionary.xaml only | Original ModAPI design (texture background) |
| Light | FluentStylesLight.xaml | Bright tone + blue accent |
| Dark | FluentStyles.xaml | Dark tone + blue accent (default) |

- Added **Theme Selector ComboBox** in Settings tab
- Theme change triggers **confirmation popup** → **auto restart**
- Theme setting saved/loaded via `theme.cfg` file

#### Hyperlink & URL Update
- Added clickable gold (`#FFD700`) hyperlink in Welcome text
- Unified all URLs to `modapi.survivetheforest.net`

#### Window Drag
- Root Grid `MouseLeftButtonDown` event for direct drag handling
- Calls `DragMove()` within top 48px area
- Drag guaranteed across all themes regardless of `AllowsTransparency` value

#### SubWindow System
- **ThemeConfirm** — Theme change confirmation popup (Yes/No)
- **ThemeRestartNotice** — Restart notice popup
- **NoProjectWarning** — No project selected warning popup
- All popups use ModAPI's own styling (no system MessageBox)

#### Caption Buttons
- Adjusted spacing and wall distance for Min/Max/Close buttons

#### Status Bar
```
Made with ♥ in Engelskirchen, Germany | upgrade by zzangae, Republic of Korea
```

---

## Localization

| Language | File |
|----------|------|
| Korean | Language.KR.xaml |
| English | Language.EN.xaml |
| German | Language.DE.xaml |
| Spanish | Language.ES.xaml |
| French | Language.FR.xaml |
| Polish | Language.PL.xaml |

---

## File Structure

```
ModAPI/
├── App.xaml / App.xaml.cs          # Theme load/save/apply
├── Dictionary.xaml                  # Original styles + Fluent fallback resources
├── FluentStyles.xaml                # Dark theme (default)
├── FluentStylesLight.xaml           # Light theme
├── ModAPI.csproj                    # Project configuration
├── Windows/
│   ├── MainWindow.xaml / .cs        # Main UI + theme selector + drag
│   ├── SplashScreen.xaml / .cs      # Splash screen
│   └── SubWindows/
│       ├── BaseSubWindow.cs         # SubWindow base class
│       ├── ThemeConfirm.xaml / .cs  # Theme change confirmation
│       ├── ThemeRestartNotice.xaml / .cs
│       └── NoProjectWarning.xaml / .cs
├── resources/
│   └── langs/
│       ├── Language.KR.xaml
│       ├── Language.EN.xaml
│       ├── Language.DE.xaml
│       ├── Language.ES.xaml
│       ├── Language.FR.xaml
│       └── Language.PL.xaml
└── libs/
    └── UnityEngine.dll              # Stub DLL
```

---

## Build Requirements

- **Visual Studio 2022**
- **.NET Framework 4.8** SDK
- **ModernWpf 0.9.6** (NuGet)

---

## License

GNU General Public License v3.0 — follows the original license.
