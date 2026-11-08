# Licensing

This repository collects patches and package recipes for several projects. Each part keeps the license of the project it changes:

| Path | License | Upstream license file |
| --- | --- | --- |
| `kernel/` | GPL-2.0 WITH Linux-syscall-note; device tree sources GPL-2.0+ OR MIT, as marked in each file | `kernel/COPYING`, `kernel/LICENSES/` (copied from the kernel tree) |
| `m1n1/` | MIT, Copyright The Asahi Linux Contributors | `m1n1/LICENSE` (copied from omacom/m1n1) |
| `omarchy/` | MIT, Copyright (c) David Heinemeier Hansson | `omarchy/LICENSE` (copied from omacom/omarchy) |
| `omarchy-settings/` | MIT, Copyright (c) David Heinemeier Hansson | `omarchy-settings/LICENSE` (copied from omacom/omarchy) |
| `omarchy-mac/` | MIT, Copyright (c) David Heinemeier Hansson | `omarchy-mac/LICENSE` (copied from omacom/omarchy-mac-pkgs) |
| `*/recipe/PKGBUILD` | MIT, as the omacom/omarchy-pkgs recipes they are based on | same as the component's MIT license |
| `README.md`, `LICENSE.md`, `CREDITS.md` | MIT | `omarchy/LICENSE` terms |

Every patch keeps its original author. Contributions from other people are redistributed under their projects' licenses.
