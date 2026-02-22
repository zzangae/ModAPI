# ModAPI(v1) v2.0.9550 - 20260223

**The Forest Mod Management Tool — Upgraded Edition**

> Original: FluffyFish / Philipp Mohrenstecher (Engelskirchen, Germany)
> Upgrade: zzangae (Republic of Korea)

---

## Overview

ModAPI is a desktop application for managing mods for The Forest. This upgraded edition includes .NET Framework 4.8 migration, Windows 11 Fluent Design UI, a 3-theme system, 13-language support, Downloads tab, and more.

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

### Phase 4 — Feature Improvements & Dead Code Cleanup

#### 4-1. Login System Cleanup
- Removed dead backend (`modapi.cc`) connection code
- `WebService.Initialize()` → no-op
- Removed 5 commented-out login handlers (ShowLoginLoader, ShowLoginUser, etc.)
- Removed active `Login()`, `DoLogout()` methods

#### 4-2. Update System Modernization
- Download URL: `modapi.cc/app/archives/` → `github.com/zzangae/ModAPI/releases/download/`
- Added GitHub redirect support (`AllowAutoRedirect` + `UserAgent`)

#### 4-3. Dead Social Links Removal
- Removed 4 SouldrinkerLP social links (Facebook, Twitter, YouTube, Twitch)

#### 4-4. Dead Code Cleanup
- Removed 6 dead Building-related methods
- Removed unused fields and using statements
- Marked `OldConfiguration`, `LanguageList` classes as `[Obsolete]`
- **MainWindow.xaml.cs**: 1127 lines → 905 lines (**-222 lines**)

#### 4-5. Developer Mode Bypass
- `--dev` command-line argument to skip game/Steam path validation
- Enables UI development without game installation

### Phase 5 — Downloads Tab & Multilingual Expansion

#### 5-1. Downloads Tab UI Framework
- Added Downloads tab to **MainWindow.xaml** (between Mods and Development)
- Internet connectivity check (`modapi.survivetheforest.net` ping)
- **Online mode**: Mod list ListView (name, author, category, download count)
- **Offline mode**: Manual download instructions + website link + retry button
- 17 Downloads-related UI string keys (all 13 languages supported)

#### 5-2. Multilingual Support Expansion (6 → 13 Languages)

| # | Code | Language | File | New |
|---|------|----------|------|-----|
| 1 | EN | English | Language.EN.xaml | |
| 2 | DE | Deutsch | Language.DE.xaml | |
| 3 | ES | Español | Language.ES.xaml | |
| 4 | FR | Français | Language.FR.xaml | |
| 5 | KO | 한국어 | Language.KR.xaml | |
| 6 | IT | Italiano | Language.IT.xaml | ✅ |
| 7 | JA | 日本語 | Language.JA.xaml | ✅ |
| 8 | PL | Polski | Language.PL.xaml | |
| 9 | PT | Português | Language.PT.xaml | ✅ |
| 10 | RU | Русский | Language.RU.xaml | ✅ |
| 11 | VI | Tiếng Việt | Language.VI.xaml | ✅ |
| 12 | ZH | 简体中文 | Language.ZH.xaml | ✅ |
| 13 | ZH-TW | 繁體中文 | Language.ZH-TW.xaml | ✅ |

- Flag icon PNGs (36×24) included for each language
- Settings language selector **custom sort order** (Korean fixed at 5th position)
- `SettingsViewModel` index mapping fix (`MainWindow.LanguageOrder`-based)
- Development selector icon loading with exception handling

---

## File Structure

```
ModAPI/
├── App.xaml / App.xaml.cs          # Theme load/save/apply, --dev mode
├── Dictionary.xaml                  # Original styles + Fluent fallback resources
├── FluentStyles.xaml                # Dark theme (default)
├── FluentStylesLight.xaml           # Light theme
├── ModAPI.csproj                    # Project configuration
├── Data/
│   └── ViewModels/
│       └── SettingsViewModel.cs     # Language/theme/settings binding
├── Windows/
│   ├── MainWindow.xaml / .cs        # Main UI + theme + drag + Downloads tab
│   ├── SplashScreen.xaml / .cs      # Splash screen
│   └── SubWindows/
│       ├── BaseSubWindow.cs         # SubWindow base class
│       ├── ThemeConfirm.xaml / .cs
│       ├── ThemeRestartNotice.xaml / .cs
│       └── NoProjectWarning.xaml / .cs
├── resources/
│   └── langs/
│       ├── Language.EN.xaml + .png
│       ├── Language.DE.xaml + .png
│       ├── Language.ES.xaml + .png
│       ├── Language.FR.xaml + .png
│       ├── Language.KR.xaml + .png
│       ├── Language.IT.xaml + .png   # New
│       ├── Language.JA.xaml + .png   # New
│       ├── Language.PL.xaml + .png
│       ├── Language.PT.xaml + .png   # New
│       ├── Language.RU.xaml + .png   # New
│       ├── Language.VI.xaml + .png   # New
│       ├── Language.ZH.xaml + .png   # New
│       └── Language.ZH-TW.xaml + .png # New
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
