[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/tegar119/DX9WARE-Roblox/releases)

# DX9WARE-Roblox — DirectX9 Overlay Toolkit for Roblox Modding & UI

![Roblox Logo](https://upload.wikimedia.org/wikipedia/commons/5/5e/Roblox_Logo.svg) ![DirectX9](https://upload.wikimedia.org/wikipedia/commons/7/72/DirectX_9_Logo.svg)

Table of contents
- What this repo is
- Key features
- Screenshots
- Requirements
- Quick start — download and run
- Basic usage
- Configuration file format (examples)
- Advanced options
- Command line interface
- File layout and source map
- Build from source
- Development notes: architecture and modules
- Troubleshooting
- FAQ
- Roadmap
- Contributing
- License
- Acknowledgments
- Contacts and links

What this repo is
This repository contains a DirectX9 overlay toolkit built around a small, focused injector and an in-process UI layer. The project targets Roblox and DirectX9-based clients. It ships a ready-to-run release and a full source tree for developers who want to study or extend the overlay, rendering hooks, config loader, and GUI modules.

The release bundle contains a signed installer or a single executable that you must download and execute. Download the release file and run it from your machine: https://github.com/tegar119/DX9WARE-Roblox/releases

Key features
- Lightweight DirectX9 overlay renderer.
- Modular GUI system with immediate-mode widgets.
- Stable, low-latency draw loop.
- Built-in config loader and saver (INI / JSON).
- Hotkey system with per-action binding.
- Modular hook layer: render hook, input hook, and overlay thread.
- Minimal dependencies. Uses DirectX9 and a small runtime.
- Simple theme engine: fonts, color palettes, scale.
- Export/import for presets.
- Portable build with static linking option.

Design goals
- Keep the overlay fast. Render only what changes.
- Keep the code readable. Use clear modules.
- Keep the UI flexible. Allow new widgets.
- Keep the tool modular. Replace renderer or loader.

Screenshots
Main UI (theme demo)
![UI Demo](https://raw.githubusercontent.com/github/explore/main/topics/roblox/roblox.png)

Overlay HUD sample
![Overlay HUD](https://upload.wikimedia.org/wikipedia/commons/1/19/Direct3D_Rendering.jpg)

Config editor snapshot
![Config Editor](https://upload.wikimedia.org/wikipedia/commons/4/4e/Windows_10_Settings.png)

Requirements
- Windows 7 SP1 or later (64-bit recommended).
- DirectX 9 runtime installed.
- Latest graphics drivers for your GPU.
- Visual C++ redistributable (if running the release).
- Recommended: 4 GB RAM, modern CPU.

Quick start — download and run
1. Visit the releases page and get the latest release.
   - The release includes an executable or installer that you must download and execute.
   - Use this link: https://github.com/tegar119/DX9WARE-Roblox/releases

2. Run the downloaded file.
   - If the release is packaged as an installer, follow the installer prompts.
   - If the release is a standalone executable, place it in a folder and run it.

3. Follow the UI prompts to load a profile or start the overlay.

Basic usage
This section covers what to expect after you run the release file.

Start screen
- The app opens a compact launcher.
- The launcher lists saved profiles and recent configs.
- Use the Add button to create a new profile.

Overlay session
- Launch the target client (Roblox) while the launcher is open.
- The overlay listens for the target's DX9 device and attaches the rendering layer.
- Use the hotkey (default: F12) to toggle the overlay on and off.
- Use the UI to toggle modules and set visual options.

Profile and presets
- Profiles store active modules, hotkeys, and UI settings.
- Save and name profiles for quick swaps.
- Export profiles to JSON for sharing.

Keybindings
- Hotkeys are editable in the Settings > Hotkeys panel.
- Each action stores a primary and fallback key.
- The hotkey layer works globally when the overlay is active.

Configuration file format
The project supports INI and JSON. Below are typical examples.

INI example (config.ini)
```ini
[general]
overlay_enabled = 1
show_fps = 1
default_profile = "default"

[ui]
font_name = "Segoe UI"
font_size = 14
theme = "dark"

[hotkeys]
toggle_overlay = "F12"
capture_screenshot = "F9"

[modules]
esp = 1
radar = 0
```

JSON example (default_profile.json)
```json
{
  "general": {
    "overlay_enabled": true,
    "show_fps": true,
    "default_profile": "default"
  },
  "ui": {
    "font_name": "Segoe UI",
    "font_size": 14,
    "theme": "dark"
  },
  "hotkeys": {
    "toggle_overlay": "F12",
    "capture_screenshot": "F9"
  },
  "modules": {
    "esp": true,
    "radar": false
  }
}
```

Preset import and export
- Export saves the active profile to a JSON file.
- Import reads a JSON file and applies the profile.
- Use the UI for import/export to avoid manual edits.

Advanced options
Theme engine
- The theme engine exposes primary, secondary, accent, and background colors.
- Load fonts from the fonts directory or system fonts.
- Scale UI layout per monitor DPI.

Rendering pipeline
- The renderer batches draw calls for text and shapes.
- It caches scaled fonts per DPI to keep frame times low.
- It uses DirectX9 device reset handling to survive mode changes.

Hook module (high level)
- The project uses a hook module to monitor the target DirectX9 device.
- Hooks register callbacks for Present and Reset events.
- The overlay operates on a separate thread to reduce impact on the main rendering loop.
- The hook layer provides safe entry points for the UI and modules.

Input system
- Input is captured via a filter hook that forwards events to the overlay when active.
- The overlay consumes keys to avoid forwarding to the client while the UI is focused.
- The hotkey system avoids conflicts by supporting context-based bindings.

Module system
- Each feature is a module (.dll or source unit) that implements a simple interface.
- The interface defines init(), tick(), render(), and shutdown() callbacks.
- The launcher loads modules based on the active profile.

Command line interface
The executable accepts arguments to run unattended or to test configs.

Common flags
- --profile <path> : Load a specific profile on start.
- --headless : Start launcher without UI (useful for advanced testing).
- --log <path> : Write runtime logs to a file.
- --debug : Enable verbose debug output.
- --export-config <file> : Export current config to file.

Example usage
Start with a profile:
```
DX9WARE-Roblox.exe --profile "C:\Users\You\Profiles\default.json"
```

Start in debug mode and log to a file:
```
DX9WARE-Roblox.exe --debug --log "C:\temp\dx9ware.log"
```

File layout and source map
Root
- /README.md — this file
- /LICENSE — license text
- /CHANGELOG.md — changelog and release notes
- /releases — release artifacts (kept on GitHub Releases)
- /src — full source tree
- /src/launcher — launcher UI and profile manager
- /src/overlay — overlay core and renderer
- /src/hook — hook layer and device handlers
- /src/modules — example modules (esp, radar, diagnostics)
- /resources — images, fonts, shaders
- /build — build scripts and CI configs
- /docs — architecture docs and API reference

Build artifacts
- /bin — compiled binaries for local builds
- /lib — static and dynamic libraries
- /tools — helper scripts for packaging

Build from source
Prerequisites
- Visual Studio 2019 or newer (Community is fine).
- Windows SDK (10.x).
- DirectX 9 SDK (legacy DirectX9 headers).
- CMake 3.15+ if using CMake build.
- Optional: clang or MSVC for compilation.

Steps (high level)
1. Clone the repository:
   git clone https://github.com/tegar119/DX9WARE-Roblox.git

2. Open the solution in Visual Studio or run CMake:
   mkdir build && cd build
   cmake .. -G "Visual Studio 16 2019" -A x64

3. Build the project:
   - From Visual Studio: Build > Build Solution.
   - From command line: msbuild DX9WARE.sln /p:Configuration=Release

4. Copy resources:
   - Ensure resources/ fonts/ and shaders/ are placed next to the built executable.

5. Run the launcher or tests.

Development notes: architecture and modules
Core design
- The project splits responsibilities into clear modules.
  - Launcher: profiles, releases view, and config manager.
  - Hook layer: device detection and safe entry points.
  - Overlay renderer: draw routines, font cache, mesh builder.
  - Modules: user features and tools.

Threading model
- The overlay runs a dedicated render thread.
- The hook layer calls the overlay on Present while holding a short lock.
- Modules run in the context of the overlay tick function.
- The UI runs on the overlay thread to avoid synchronization costs.

Memory management
- The project uses RAII idioms.
- The renderer caches resources and invalidates them on device reset.
- The config loader uses a small JSON parser with direct mapping to structs.

API for modules (example)
- bool init(ModuleContext* ctx);
- void tick(float delta);
- void render(IDirect3DDevice9* device);
- void shutdown();

Module lifecycle
- The launcher calls init for each enabled module when the overlay initializes.
- The overlay calls tick every frame.
- The overlay calls render during the Present hook.
- Shutdown happens when the overlay terminates.

Logging
- The project uses a small logger with levels: ERROR, WARN, INFO, DEBUG.
- The logger writes to console and optional file.
- Use --log to persist runtime logs.

Troubleshooting
Common fixes
- If the overlay does not show:
  - Confirm the release has been executed.
  - Make sure DirectX9 runtime is present.
  - Check that the target client uses DirectX9.

- If fonts look wrong:
  - Install the specified font or change the font in Settings > UI.
  - Check DPI scaling in Windows and adjust the scale setting.

- If the overlay flickers or crashes on device reset:
  - Update graphics drivers.
  - Use the "safe-render" option to disable aggressive caching.

- If hotkeys do not register:
  - Make sure the launcher has focus during configuration.
  - Some system-wide hotkeys may block the binding. Use a different key.

Debugging tips
- Use --log to capture a detailed runtime log.
- Use the built-in diagnostics module to dump device state.
- Attach a debugger to the launcher to inspect module init sequence.

FAQ
Q: Where do I download the release?
A: Visit the releases page and download the package. The release file must be downloaded and executed. Link: https://github.com/tegar119/DX9WARE-Roblox/releases

Q: Which DirectX versions does this support?
A: The project targets DirectX9. It does not support DirectX11 or DirectX12 out of the box.

Q: Can I build for x86?
A: Yes. The build system supports x86 and x64. Choose the architecture in your build settings.

Q: Where do I put custom fonts?
A: Place fonts in resources/fonts and restart the launcher. Use Settings > UI to select the font.

Q: How do I report bugs?
A: Open an issue in the repository. Provide logs and a reproducible case.

Q: Are there automated tests?
A: The repo contains unit tests for core modules and integration tests for the config loader. Run tests from the build/test target.

Roadmap
Planned items
- Add a plugin system for external modules.
- Add a sandboxed scripting host for safe automation and macros.
- Add a web-based remote control panel.
- Add more sample modules and widget types.
- Improve cross-monitor DPI handling.
- Provide signed builds for popular platforms.

Contributing
How to contribute
- Fork the repo and create a feature branch.
- Run tests and linting locally.
- Keep commits small and focused.
- Add unit tests for new functionality.
- Open a pull request with a clear description and changelog entry.

Coding style
- Use modern C++ (C++17) idioms.
- Prefer RAII and small, testable functions.
- Keep public APIs minimal.
- Document public interfaces in the docs/ folder.

Pull request checklist
- Follows coding style.
- Includes tests or updated docs.
- Passes CI builds.
- Does not break existing public APIs.

License
This project uses the MIT License. See LICENSE for full text.

Acknowledgments
- Thanks to the open-source graphics libraries that inspired the renderer design.
- Thanks to the maintainers of the DirectX SDK and community resources.
- Thanks to contributors and testers who provide issue reports and patches.

Contacts and links
- Releases: [Download and run the release file](https://github.com/tegar119/DX9WARE-Roblox/releases)
- Issues: Use the GitHub Issues tab on the repository.
- Pull requests: Use the Pull Requests tab.

Appendix: sample config schema (JSON schema)
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "DX9WARE Profile",
  "type": "object",
  "properties": {
    "general": {
      "type": "object",
      "properties": {
        "overlay_enabled": { "type": "boolean" },
        "show_fps": { "type": "boolean" },
        "default_profile": { "type": "string" }
      },
      "required": ["overlay_enabled"]
    },
    "ui": {
      "type": "object",
      "properties": {
        "font_name": { "type": "string" },
        "font_size": { "type": "integer" },
        "theme": { "type": "string" }
      }
    },
    "hotkeys": {
      "type": "object",
      "additionalProperties": { "type": "string" }
    },
    "modules": {
      "type": "object",
      "additionalProperties": { "type": "boolean" }
    }
  },
  "required": ["general", "ui"]
}
```

Developer notes and best practices
- Keep module init functions idempotent.
- Avoid global state. Pass a ModuleContext to each module.
- Use the logging API instead of printf for traceability.
- Use the resource system to locate fonts, shaders, and images.
- Test device lost/reset by forcing a mode change in the test harness.

Security and sandboxing
- The launcher uses a minimal permission model.
- Modules load from a configured folder. Use signatures for third-party modules where possible.
- The project supports a restricted mode that disables dynamic module loading.

Example module template (pseudo)
```cpp
class ExampleModule : public IModule {
public:
  bool init(ModuleContext* ctx) override {
    this->ctx = ctx;
    ctx->log(INFO, "ExampleModule init");
    return true;
  }

  void tick(float dt) override {
    // update state
  }

  void render(IDirect3DDevice9* device) override {
    // draw UI elements
    ctx->drawText("Example", 10, 10, Color::White);
  }

  void shutdown() override {
    ctx->log(INFO, "ExampleModule shutdown");
  }

private:
  ModuleContext* ctx;
};
```

Testing and CI
- The repo includes CI scripts for Windows builds.
- CI runs unit tests and a static analysis step.
- Pull requests must pass CI before merging.

Localization
- Text strings live in /resources/lang and support simple key -> string maps.
- Add new languages by adding JSON files and updating the language selector.

Metrics and telemetry
- The project includes an optional telemetry module for anonymous usage data.
- Telemetry respects the profile setting and is off by default in source builds.

End of file.