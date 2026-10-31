# Creative Sound Blaster Control Center — Headphone & Speaker Switcher

**One-click (or one-hotkey) switching between Speakers and Headphones on Creative Sound Blaster sound cards.**

Tired of opening the Creative app every time you plug or unplug a headset? This app sits in your system tray and flips your Sound Blaster's speaker configuration between **Speakers** and **Headphones** instantly — via a tray menu, a button, or a global hotkey from anywhere.

<p align="center">
  <img src="https://img.shields.io/badge/platform-Windows%20x86-blue" alt="Platform: Windows x86"/>
  <img src="https://img.shields.io/badge/runtime-.NET%20Framework%204.x-blueviolet" alt="Runtime: .NET Framework 4.x"/>
  <img src="https://img.shields.io/github/v/release/symshah/creative-sound-blaster-headphone-speaker-switcher" alt="Release"/>
  <img src="https://img.shields.io/github/license/symshah/creative-sound-blaster-headphone-speaker-switcher" alt="License"/>
</p>

---

## Why?

Sound Blaster cards expose a **speaker configuration** (2.0 speakers vs. headphones) in the Creative audio console. Switching it manually for every headset swap is annoying. The switcher wraps the card's native **Sound Core (CtSndCr)** control API and does it for you in one click — no Creative software window needed.

## Features

- 🎧 **Toggle Speakers ⇄ Headphones** — drives the card's real speaker-config register through the Creative `CTSoundCore` COM interface (no virtual-audio hacks)
- ⌨️ **Global hotkey** — toggle from any app, even when the window is hidden to the tray
- 🔧 **Fully configurable hotkey** — any modifier combo (Ctrl / Alt / Shift / Win) + key, saved to `hotkey.txt` next to the exe
- 🖥️ **System tray app** — right-click menu: *Show window*, *Switch to Speakers*, *Switch to Headphones*
- 📟 **Live state label** — shows the current output (`SPEAKERS` / `HEADPHONES`) as detected from the card
- 🪶 **Tiny & portable** — a single ~14 KB exe + one interop DLL, no installer

## Requirements

| Item | Requirement |
|---|---|
| OS | Windows 10 / 11 (32‑bit process — runs on x64 Windows) |
| Sound card | Creative Sound Blaster with the **CtrlSndCr / Sound Core** control layer (e.g. Sound Blaster Z, Zx, AE‑5/7/9, X series, G-series) |
| Runtime | .NET Framework 4.x (built into Windows 10/11) |
| Privileges | Normal user is fine; run as Administrator if toggle fails with a permissions error |

## Installation

1. Download the latest `SbxSwitcher.zip` from the [Releases](https://github.com/symshah/creative-sound-blaster-headphone-speaker-switcher/releases) page.
2. Extract both files (`SbxSwitcher.exe` + `Interop.CtSndCr.dll`) into a folder — they must stay together.
3. Run `SbxSwitcher.exe`.
   - Windows SmartScreen may warn about an unsigned app — click **More info → Run anyway**.
4. The app starts in the tray. Open the window to check the current output and set your hotkey.

> Make sure your Creative drivers / Sound Blaster Command are installed — the app talks to the card through them.

## Usage

### Toggle with the window

- Click **Toggle** — the state label flips between `SPEAKERS` and `HEADPHONES`.
- Or use the tray icon menu → *Switch to Speakers* / *Switch to Headphones*.

### Set a global hotkey

1. Open the switcher window.
2. Click the **hotkey** button.
3. Press the modifier(s) + key you want (e.g. `Ctrl + Alt + H`).
4. Done — the combo works system-wide. It is stored in `hotkey.txt` in the app folder (portable across machines / reinstall).

### Auto-detect

The app polls the card's headphone detection (`IsHeadphones`) so the status label always reflects what the card actually reports — plug in your headphones and watch it flip.

## How it works

The app is a thin WinForms wrapper over the **Creative Sound Core COM API** (`Interop.CtSndCr.dll` → `CTSoundCore` CLSID `{3C0E7BA7-F9C3-460F-BCBE-FC91A06EF3F3}`):

- `ReadSpeakerConfig()` / `Write()` — set the card's speaker configuration to `SPEAKERS_CFG` or `HEADPHONES_CFG`
- `IsHeadphones()` — query the current physical output state
- Global hotkeys via the Win32 `RegisterHotKey` API with `MOD_NOREPEAT`

It does **not** use software virtual-cable tricks or touch Windows audio endpoint routing — the switch happens inside the Sound Blaster hardware/DSP, so apps don't restart or drop audio.

## Troubleshooting

| Problem | Fix |
|---|---|
| *"SoundCore init failed"* on start | Creative drivers not installed, card not detected, or app not running as admin. Install Sound Blaster Command and retry. |
| Hotkey doesn't respond | Another app registered the same combo. Change it in the app. |
| Toggle has no effect | Ensure this is the active playback device in Windows Sound settings; run the exe as Administrator. |
| Where is my config? | `hotkey.txt`, in the same folder as `SbxSwitcher.exe`. |

## Building from source

This repository currently distributes the official **prebuilt binaries** (release builds). The full C# source is kept in the author's workspace — open an issue if you want it published here.

## Disclaimer

This is an independent fan-made tool and is **not affiliated with, endorsed by, or connected to Creative Technology Ltd.** All product names and APIs belong to their respective owners. Use at your own risk — this tool writes parameters to your sound card's control interface.

## License

[MIT](LICENSE) © Masood Shah
