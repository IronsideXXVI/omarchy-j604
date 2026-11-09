# Changes that are not installed

The J604 runs the stock Omarchy **edge** userland (`omarchy-dev` and `omarchy-settings-dev` from the official recipes), with two local additions: the keyboard-backlight series in `omarchy-dev/` and `omarchy-settings-dev/`, and the built-in panel defaults in `omarchy-settings-dev/` and `omarchy-mac/`. Everything on this page is **not installed** and **not in this tree**. Nothing was deleted from history: commit 96827b7 (the initial import) holds the last published version of most of it, in `omarchy/patches/` and `omarchy-settings/patches/` (built as local pkgrel 3.610 on Omarchy 4.5 then).

| Topic | What it did | Where it is now |
| --- | --- | --- |
| Automatic display brightness | Brightness followed the built-in light sensor | Not shipped. 96827b7: `omarchy/patches/0009`, `0010`, `0011`; `omarchy-settings/patches/0006` |
| Glassy bar blur ("xray") | Blur the Glassy bar from the cached wallpaper blur (59 to 119 fps on this panel) | Not in our packages. Open upstream as [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176). 96827b7: `omarchy-settings/patches/0005`, `omarchy/patches/0008` |
| Boot splash handover | Fade from the boot splash into the desktop without a black frame | Not installed. 96827b7: `omarchy/patches/0004`, `0005`; `omarchy-settings/patches/0003`, `0004`. The boot-package part (external displays released after the splash) still ships in `omarchy-mac-boot/` |
| Kyber retry | Retry the kyber scheduler write while a sibling disk switches | Not installed. Open upstream as [omacom/omarchy#18228](https://github.com/omacom/omarchy/pull/18228). 96827b7: `omarchy-settings/patches/0001` |
| Battery charge threshold | Read the battery's own charge threshold | Not installed. 96827b7: `omarchy/patches/0007` |
| Custom repository hook | Put the add-custom-repo hook into use | Not installed. 96827b7: `omarchy-settings/patches/0002` |
| Internal display defaults (`apple-display.lua`) | J604 panel defaults (bit depth, colour management, adaptive sync) | Never published here and not installed. Replaced on 2026-11-09 by the installed panel defaults (10-bit, Display P3; VRR left off) in `omarchy-settings-dev/` 1.4 and `omarchy-mac/` 2.12 |
| Neural Engine userland rebuilds | Local rebuilds of the ANE packages | Withdrawn; the J604 uses the official edge builds of `omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler` and `omarchy-mac-ml`. Never published here |

Two earlier topics are installed again, in a new form:

- **Keyboard backlight idle timeout** (96827b7: `omarchy/patches/0001`-`0003`): now the four-patch series in `omarchy-dev/` and `omarchy-settings-dev/` on the edge recipes, with the `omarchy-mac` side in `omarchy-mac/` (0001).
- **Keybinding key names** (96827b7: `omarchy/patches/0006`, [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177), closed without merging): now `kbd-0104` in the same series.
