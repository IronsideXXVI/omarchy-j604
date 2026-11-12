# Credits

Patches in this repository by author (from each patch's `From:` line). Many are other people's work,
carried unchanged with their authorship from Aurora Silicon, the Asahi Linux project, Omarchy and the
Omarchy for Apple Silicon contributors. Thank you.

Since kernel 7.3 (local 1.703) the kernel series is 163 patches instead of 766 (164 since 1.704, 175 since 1.705, 176 since 1.706, 188 since 1.710, 194 since 1.711, 195 since 1.712, 197 since 1.713, 199 since 1.714, 200 since 1.715, 203 since 1.716, 204 since 1.717, 206 since 1.718, 207 since 1.719, 212 since 1.720, 215 since 1.721, 218 since 1.722, 219 since 1.723, 220 since 1.724, 221 since 1.725, 222 since 1.726, 225 since 1.727, 228 since 1.728, 229 since 1.729, 230 since 1.730, 231 since 1.731, 232 since 1.732, 236 since 1.733, 239 since 1.734, 245 since 1.736, 227 since 1.800, 228 since 1.802, 231 since 1.803, 233 since 1.804, 236 since 1.805, 237 since 1.807, 240 since 1.808, 245 since 1.809, 246 since 1.810, 247 since 1.811, 249 since 1.813, 252 since 1.816, 264 since 1.820, 276 since 1.821, 279 since 1.823, 283 since 1.826, 289 since 1.829, 295 since 1.832, 297 since 1.833, 303 since 1.836, 311 since 1.841, 320 since 1.843, 323 since 1.844, 329 since 1.845, 330 since 1.846, 336 since 1.847, 337 since 1.848, 338 since 1.850): Aurora's 7.3 base already
contains much of what we carried before, under its authors' names there. Kernel patch 0157 folds the rest
of the 1.617 J604 series into one integration commit; its authors are those of the 1.617 series, which
the history of `kernel/` keeps (kernel 1.617 was added in commit 4407a2a). With 1.800 the series moved to a newer
Aurora base, and 19 patches it now contains left the series (`kernel/RENUMBER.md`); they are credited there under
their authors' names.

| Author | Patches |
| --- | --- |
| Dylan Ironside | 335 |
| Ryan Murray | 24 |
| Diljit Singh | 23 |
| RJ Skerry-Ryan | 8 |
| Bartosz Nitka | 7 |
| Erik Bourget | 7 |
| Friedrich | 2 |
| Lucas Goossen | 2 |
| Naeem Malik | 2 |
| Satya Benson | 3 |
| Daniel Fernau | 1 |
| DjDeveloperr | 2 |
| Eryk Wieliczko | 1 |
| Janne Grunau | 1 |
| Jonathan Goyvaerts | 6 |
| Rolando Trevino | 1 |
| Sean Dolbec | 1 |
| ewb | 1 |

Also credited in a patch message: the built-in panel defaults (`omarchy-settings-dev/patches/disp-0201`) follow omacom/omarchy#10549 by j-c-m, which set `cm = "dp3"` for the Dell XPS OLED panel.

`libfprint-git/patches/aurora-touchid-v5.patch` carries patches 0004 and 0005 from aurora-silicon/aurora-sep-userspace (shipped in aurora-silicon/alarm-aurora `libfprint-aurora`), renamed to the omacom driver's names. The patch has no single `From:` author, so it is not counted in the table.

`libva-v4l2_request-avd/patches/recipe-aurora-alias.patch` is a recipe diff that carries Ryan Murray's alias change (aurora-silicon/alarm-aurora aedcf240ca91, also proposed for omacom as omacom/omarchy-pkgs#2375 by edimoldovan). It has no `From:` line, so it is not counted in the table.
