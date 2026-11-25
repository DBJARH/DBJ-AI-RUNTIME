# Xubuntu desktop setup

Machine: Xubuntu 26.04 LTS, Xfce 4.20 on X11.
Displays: laptop eDP-1 1920x1080 (~142 dpi) + external HDMI-1 2560x1440 (~109 dpi).

Note: Claude's shell has no sudo — hand the user sudo commands to run rather than running them directly.

## Fonts / DPI (settled 2026-11-25, user confirmed laptop finally readable)

- Font: Liberation Sans 12 (Arial-metric substitute; real Arial not installed)
- Title font: Liberation Sans Bold 12
- Xft DPI: 120
- Originally Noto Sans 9 @ 96 dpi — unreadable on the laptop screen.

Set via Settings > Appearance (Fonts tab) and Settings > Appearance > Fonts > Custom DPI, or directly in `~/.config/xfce4/xfconf/xfce-perchannel-xml/xsettings.xml`.

## No backdrop/inactive-window fading

User dislikes Xfce fading inactive windows. Fix: override Greybird's `:backdrop` colors in `~/.config/gtk-3.0/gtk.css`.

## Window manager theme (settled 2026-11-25)

Titlebar buttons (minimize/maximize/close) are too small under Greybird at this DPI. Fix: Settings > Window Manager > Style tab — button size scales with the "Title font" size and with the chosen theme. User tried Default-hdpi, Default-xhdpi, Numix; settled on **Default-xhdpi**.

## Screenshot shortcut

`Shift+Super+S` → `xfce4-screenshooter -r -c` (region capture to clipboard, Windows-style). Bind under Settings > Keyboard > Application Shortcuts.

## General note

Ask the user before quitting their running apps in order to apply any of these settings.
