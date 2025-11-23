# W.O.P.R. – Steam Gaming Mode Launcher for OMARCHY (Arch)

> *“Shall we play a game?”*

W.O.P.R. is an opinionated Steam “gaming mode” launcher for Arch-based systems running **Hyprland**.

It installs all the bits you need for a SteamOS-style experience (Steam, Gamescope, MangoHud, GameMode, 32-bit libs, GPU drivers), wires everything into Hyprland keybinds, and gives you a friendly TUI to pick resolution and performance overlay before it launches Steam.

Once installed:

- `SUPER`+`SHIFT`+`S` → **enter Gaming Mode**  
- `SUPER`+`SHIFT`+`R` → **exit Gaming Mode and restore your desktop**

---

## Features

- 🎮 **One-key Steam “Gaming Mode”**
  - Launches Steam inside Gamescope using a floating, pinned TUI
  - Default keybinds: `SUPER`+`SHIFT`+`S` to start, `SUPER`+`SHIFT`+`R` to exit
- 🖥️ **Resolution presets & scaling**
  - Native panel resolution
  - 1080p → 1440p upscaled
  - Native 1440p
  - 1440p → 4K upscaled
  - Native 4K/UHD (3840×2160)
- 📊 **MangoHud presets**
  - Off (clean view)
  - Minimal – FPS counter only
  - Full stats – CPU, GPU, frametime, temps, etc.
- ⚙️ **Automatic GPU detection**
  - NVIDIA, AMD, Intel iGPU, Intel Arc
  - Installs appropriate Vulkan and 32-bit driver stacks via `pacman`
- 🚀 **Performance mode (optional)**
  - Temporarily switches CPU governor and GPU power/performance settings for gaming
  - Uses udev rules so later launches do **not** require sudo
  - Restores everything cleanly when you exit gaming mode
- 🧠 **Remembers your choices**
  - Saves last used resolution and overlay and offers to reuse them next time
- 🎨 **Themed TUI with `gum`**
  - Uses your terminal’s colours (Ghostty/kitty/Alacritty/foot/xterm)
  - Clean centred layout with a WarGames-style header
- 🧩 **Intel Arc quality-of-life fixes**
  - Optional GTK4 rendering workaround (sets `GSK_RENDERER=gl`) to fix visual glitches in Nautilus and other GTK4 apps
- 🧰 **Arch-friendly installer**
  - Checks multilib, Steam dependencies and 32-bit libs
  - Offers a full system upgrade (`pacman -Syu`) before installing anything
  - Rolls back cleanly if something fails

---

