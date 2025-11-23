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
