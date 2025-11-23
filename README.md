# W.O.P.R – Steam Gaming Mode for Hyprland

> “Shall we play a game?”

**W.O.P.R** is a Hyprland-centric “gaming mode” launcher for Arch-based systems (designed on **Omarchy**).  
It installs a pair of helper scripts and Hyprland keybinds that:

- Start **gamescope** in a full-screen gaming session
- Launch **Steam** in Big Picture / Gamepad UI mode
- Apply CPU & GPU performance tweaks
- Optionally show a **MangoHud** overlay
- Restore everything cleanly when you quit

Once installed you get:

- `Super + Shift + S` → **Enter Gaming Mode** (W.O.P.R TUI, then Steam)
- `Super + Shift + R` → **Exit Gaming Mode** (restore governors, GPU states, idle, etc.)

---

## Features

- 🖥 **Resolution presets**

  The TUI detects your current monitor and lets you choose:

  - Native resolution (best quality)
  - 1440p upscaled (1080p → 1440p)
  - 1440p native (2560×1440)
  - 4K upscaled (1440p → 4K)
  - 4K/UHD native (3840×2160)

  Internally it configures *gamescope*’s render and output sizes to match.

- 📊 **MangoHud presets**

  - **Off** – cleanest view  
  - **Minimal** – FPS counter only  
  - **Full Stats** – FPS, frametime, CPU/GPU temps, power, RAM, etc.

- ⚙️ **Performance mode**

  When `PERFORMANCE_MODE=enabled` (default):

  - Uses **gamemoderun** to wrap gamescope/Steam
  - Can add `--rt --immediate-flips` to gamescope for lower latency
  - Optionally grants `cap_sys_nice` to `gamescope` (if you accept the prompt)
  - Switches CPU governor to performance-oriented mode while gaming
  - Applies GPU performance tweaks and records original settings

  On exit, CPU governor, GPU power/perf modes and idle behaviour are restored.

- 🎮 **Steam launch modes**

  Configurable via `/etc/gaming-mode.conf` or `~/.gaming-mode.conf`:

  - `STEAM_LAUNCH_MODE=bigpicture` → `steam -tenfoot` (default)
  - `STEAM_LAUNCH_MODE=gamepadui` → `steam -gamepadui`

- 🧠 **Smart GPU support**

  Detects GPU(s) via `lspci` and helper functions:

  - **NVIDIA** – installs Vulkan loader, 32-bit libs and video drivers
  - **AMD** – installs RADV Vulkan stack, 32-bit libs and VAAPI/VDPAU
  - **Intel Arc (xe)** – installs lean Vulkan + compute stack and applies
    a `GSK_RENDERER=gl` fix for GTK4 apps on Arc
  - **Intel iGPU (i915/Xe)** – installs the full Intel media/Vulkan stack

  On first run it can also add you to relevant groups (e.g. `video`, `input`,
  `gamemode` etc.), and will warn that you need to log out and back in.

- 🎨 **Omarchy-aware theming**

  If available, it reads colours from:

  - `~/.config/omarchy/current/theme/ghostty.conf`
  - `~/.config/omarchy/current/theme/colors.sh`

  and uses them to style the **gum** TUI. If those aren’t present, it falls
  back to a neutral dark theme.

- 🪟 **Tidy Hyprland integration**

  - Automatically finds your Hyprland config:
    - `~/.config/hypr/bindings.conf`
    - `~/.config/hypr/keybinds.conf`
    - `~/.config/hypr/hyprland.conf`
  - Appends a clearly-marked block of bindings and window rules
  - Makes the W.O.P.R terminal **floating, centred and pinned**

---

## How it works

The installer script (`WOPR.sh`):

1. **Validates environment**

   - Requires `pacman`, `hyprctl` and a Hyprland config under `~/.config/hypr/`.

2. **Checks Steam + GPU dependencies**

   - Refreshes pacman DB (`pacman -Syy`)
   - Optionally offers a full system upgrade (`pacman -Syu`)
   - Verifies the **multilib** repository is enabled (needed for 32-bit Steam)
   - Assembles a list of core packages (Steam, Vulkan loaders, 32-bit libs etc.)
     plus GPU-specific packages, then offers to install anything missing.

3. **Installs core tools (if needed)**

   Through `setup_requirements()` it ensures:

   - `steam`, `gamescope`, `mangohud`, `gum`
   - `python`, `pciutils`, `libcap`, `gamemode`
   - plus whatever is needed by your GPU

4. **Deploys launcher scripts**

   Creates:

   - `~/.local/share/steam-launcher/enter-gamesmode`
   - `~/.local/share/steam-launcher/leave-gamesmode`

   These manage:

   - Session state (`~/.cache/gaming-session`)
   - CPU governor / GPU perf modes
   - Hypridle (suspend/lock) behaviour
   - gamescope + Steam start/stop
   - MangoHud configuration

5. **Adds Hyprland keybinds**

   Appends a block like:

   ```ini
   # Gaming Mode bindings - added by installation script
   windowrulev2 = float, class:(WOPR-Terminal)
   windowrulev2 = size 800 600, class:(WOPR-Terminal)
   windowrulev2 = center, class:(WOPR-Terminal)
   windowrulev2 = pin, class:(WOPR-Terminal)

   bindd = SUPER SHIFT, S, Steam Gaming Mode, exec, <your-terminal> ~/.local/share/steam-launcher/enter-gamesmode
   bindd = SUPER SHIFT, R, Exit Gaming Mode,   exec, ~/.local/share/steam-launcher/leave-gamesmode
   # End Gaming Mode bindings

   (It auto-detects whether your config uses bind or bindd.)


	
   
6.	Validation

	A final check confirms:
	•	Launcher scripts exist and are executable
	•	The bindings block is present in your Hypr config
	•	gamescope capabilities are compatible with PERFORMANCE_MODE
