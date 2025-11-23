# Troubleshooting

A few common issues and fixes when using W.O.P.R.

---

## Gamescope fails to start

**Symptoms**

- You see “ERROR: Gamescope failed to start!”
- Gaming Mode exits immediately after the spinner

**Things to check**

1. Run `gamescope --help` from a terminal to confirm it is installed.
2. Make sure your GPU has working Vulkan support:
   - `vulkaninfo | less` (from `vulkan-tools`)
3. Confirm you’re on a supported GPU driver:
   - NVIDIA: `nvidia`, `nvidia-dkms`, or `nvidia-open-dkms`
   - AMD: `amdgpu` with `vulkan-radeon`
   - Intel / Arc: recent Mesa recommended

Re-run `./WOPR.sh` to let it re-check dependencies if needed.

---

## Keybinds don’t work

**Symptoms**

- `SUPER`+`SHIFT`+`S` does nothing

**Checks**

1. Open your Hypr config (e.g. `~/.config/hypr/bindings.conf`).
2. Look for the block:

   ```ini
   # Gaming Mode bindings - added by installation script
   ...
   # End Gaming Mode bindings




3.	Restart Hyprland or reload config: hyprctl reload.

If you use a custom binding file layout, make sure the file W.O.P.R. edited is actually included from your main Hypr config.


Steam opens but not in Big Picture

Check ~/.gaming-mode.conf:

STEAM_LAUNCH_MODE=bigpicture

Only two modes are currently supported:
	•	bigpicture (default)
	•	gamepadui


Performance tweaks don’t seem to apply

Ensure you allowed W.O.P.R. to create the performance udev rules:
	•	Check for /etc/udev/rules.d/99-gaming-performance.rules.

If you skipped this during installation:
	1.	Re-run ./WOPR.sh.
	2.	When prompted about passwordless performance controls, answer Y.
	3.	Reboot or log out/in to let udev rules take effect.


   Intel Arc GTK apps look broken

If you skipped the Intel Arc workaround during install you can run ./WOPR.sh again and accept the Intel Arc GTK4 rendering fix prompt, or manually set:

GSK_RENDERER=gl

in an environment file under /etc/environment.d/ or ~/.config/environment.d/.


If you hit something else, please open an issue with:
	•	Distro + kernel
	•	GPU model
	•	Output of ./WOPR.sh (or at least the last 30–40 lines)
	•	Any relevant Hypr config fragments


