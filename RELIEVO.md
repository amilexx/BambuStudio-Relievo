# Bambu Studio — version patchée Relievo

**Base :** Bambu Studio `v02.07.01.62` (tag officiel de [bambulab/BambuStudio](https://github.com/bambulab/BambuStudio)).
**Modifications :** Relievo (relievo.fr), de juillet à novembre 2026. Cette branche contient le tag d'origine plus les commits
Relievo, un par modification ; `git diff v02.07.01.62` les montre en entier (18 fichiers de `src`, environ 400 lignes).
**Licence :** inchangée, **GNU AGPL v3** (voir `LICENSE`). Ce dépôt **est** la source correspondante du binaire publié dans les
*Releases*.

Relievo fabrique des cartes en relief imprimées en 3D, en plusieurs couleurs. Ces modifications servent ce travail ; elles
n'ont **aucun lien** avec Bambu Lab, qui ne les a ni relues ni approuvées.

## Ce qui change

| # | Fichier(s) | Changement |
|---|---|---|
| 1 | `PrintConfig.hpp/.cpp`, `Preset.cpp`, `Tab.cpp`, `MultiMaterialSegmentation.cpp` | Nouvelle option **`color_penetration_taper`** (« Taper paint penetration », onglet *Strength*), **activée par défaut = comportement d'origine**. Désactivée, la zone peinte n'est plus rétrécie d'une ligne d'extrusion à chaque couche de pénétration : la couleur descend à pleine largeur (un prisme au lieu d'un cône). Les fines routes ou rivières peintes gardent ainsi leur couleur sur toute la profondeur. Les régions trop fines pour être imprimées restent écartées par l'`opening_ex()` d'origine. |
| 2 | `PrintConfig.cpp`, `Tab.cpp` | `mmu_segmented_region_max_width` : valeur par défaut de **2 mm** (au lieu de 0 = désactivé), et réglage **réaffiché** dans l'interface. Masqué, il n'était retenu ni par un preset ni par un projet. |
| 3 | `PrintObjectSlice.cpp` | Retrait des **îlots résiduels** (< 0,2 mm², réglable par la variable d'environnement `RELIEVO_MIN_ISLAND_MM2`, `0` = désactivé) laissés par la différence entre volumes quasi coïncidents. Sans ce filtre, ils recevaient des périmètres parasites. |
| 4 | `Tab.cpp` | Le mode de purge **« Prime Saving »** est proposé sur toute machine multi-extrudeur (la X2D déclare `extruder_max_nozzle_count=[1,1]` et le bouton restait caché). |
| 5 | `src/CMakeLists.txt` | Ajout de l'exécutable **`bambu-studio-console.exe`** (même programme, en mode console), pour piloter le tranchage par script. |
| 6 | `CMakeLists.txt`, `src/slic3r/CMakeLists.txt`, `build_win.bat` | Compilation Windows avec les seuls *Build Tools* de Visual Studio et sans `pkg-config` : `vswhere -products *`, `PkgConfig` facultatif sous Windows, FFmpeg résolu directement. |
| 7 | `PrintObject.cpp`, `Layer.hpp`, `Fill/Fill.cpp`, `Fill/FillBase.hpp`, `Fill/FillRectilinear.cpp` | **Cœur léger** (novembre 2026), **désactivé par défaut**. Avec la variable d'environnement `RELIEVO_CORE_MARGIN_MM` (en mm), le remplissage clairsemé en grille garde sa densité à moins de cette distance de toute surface (dessus, dessous, flancs, cavités) ; au-delà, une ligne de grille sur `RELIEVO_CORE_KEEP` seulement (3 par défaut). Les lignes gardées traversent le cœur sans être coupées. `RELIEVO_CORE_LINK=1` relie en plus les bouts coupés le long du bord du cœur. |
| 8 | `Print.cpp`, `GCode.cpp/.hpp`, `GCode/ToolOrdering.hpp` | **Purge dans le remplissage sans tour de purge** (novembre 2026), **désactivée par défaut**. Avec `RELIEVO_FLUSH_INFILL_NO_TOWER=1` et *Flush into objects' infill* sur l'objet, une partie de la purge part dans le remplissage interne de la couche ; la purge à la goulotte ne descend jamais sous `RELIEVO_FLUSH_MIN_MM3` (45 mm³ par défaut). |
| 9 | `Geometry/Voronoi.cpp` | **Tranchage qui ne finit jamais** (novembre 2026). La réparation d'un diagramme de Voronoi dégénéré tourne les segments puis arrondit leurs extrémités : deux segments peuvent alors se croiser, et `boost::polygon::construct_voronoi` ne revient plus (vu sur les couches à zones peintes : console bloquée sur un cœur). L'entrée tournée est maintenant vérifiée ; un angle qui la rend invalide est sauté et l'angle suivant essayé. Sur un plateau qui calait 6 fois sur 6 : 16 tranchages sur 18 finissent, résultats identiques. |
| 10 | `MultiMaterialSegmentation.cpp` | **Zones peintes découpées proprement** (novembre 2026). Les traits peints projetés sur un bord de couche pouvaient se chevaucher ou pointer à rebours ; le bord découpé revenait alors sur lui-même, ce que le calcul de Voronoi ne supporte pas : diagramme dégénéré, îlots entiers d'une couche du dessus coloriés au hasard d'un tranchage à l'autre, et parfois tranchage sans fin. Les morceaux de chaque bord sont maintenant reconstruits dans l'ordre, du début à la fin du bord. Sur un plateau d'essai : entrées invalides 49 → 4, réparations ratées 47 → 2, même filament par couleur d'un tranchage à l'autre. `RELIEVO_MM_NO_MONOTONIC=1` l'éteint (comparaisons). |

## Compiler

Comme l'original (voir `doc/` et le README d'origine) : dépendances avec `build_win.bat -d <dossier_deps> -s deps`, puis
l'application avec `build_win.bat -d <dossier_deps> -s app`. Depuis novembre 2026, les binaires publiés sont compilés par
GitHub Actions (`.github/workflows/relievo_win.yml`, `RelWithDebInfo`, MSVC 2022) ; chaque Release donne son commit et son run.

## Binaire portable (Releases)

Zip à décompresser, puis lancer `bambu-studio.exe` (interface) ou `bambu-studio-console.exe` (ligne de commande).
- Le dossier `resources` est une **copie complète**. Dans l'arbre de compilation, c'est une jonction vers les sources.
- Le runtime **Visual C++ 2015-2022 (x64)** est joint (DLL redistribuables de Microsoft). Sous Windows 10, l'interface demande
  aussi le runtime **WebView2**, présent d'office sous Windows 11.
- Les profils et réglages sont partagés avec un Bambu Studio officiel installé (`%APPDATA%\BambuStudio`).

---

# Bambu Studio — Relievo patched build (English)

**Base:** Bambu Studio `v02.07.01.62` (official tag of [bambulab/BambuStudio](https://github.com/bambulab/BambuStudio)).
**Modified by:** Relievo (relievo.fr), July–November 2026. This branch is the original tag plus the Relievo commits, one per
change; `git diff v02.07.01.62` shows all of them (18 files under `src`, ~400 lines).
**License:** unchanged, **GNU AGPL v3** (see `LICENSE`). This repository **is** the corresponding source of the binary
published under *Releases*.

Relievo makes multi-colour 3D-printed relief maps; these changes serve that work and are **not affiliated** with Bambu Lab,
which has neither reviewed nor endorsed them.

1. New option **`color_penetration_taper`** ("Taper paint penetration", *Strength* tab), **on by default = upstream
   behaviour**. When off, the painted region is no longer shrunk by one extrusion line per penetration layer: paint goes
   straight down (a prism instead of a cone), so thin painted roads/rivers keep their colour through the whole depth. Regions
   too thin to print are still removed by the upstream `opening_ex()`.
2. `mmu_segmented_region_max_width`: default **2 mm** (was 0 = off) and the setting is **shown again** in the UI (hidden, it
   was not kept by presets or projects).
3. Removal of **residual islands** (< 0.2 mm², env var `RELIEVO_MIN_ISLAND_MM2`, `0` = off) left by the difference between
   near-coincident volumes, which otherwise got spurious perimeters.
4. **"Prime Saving"** purge mode offered on any multi-extruder machine (the X2D reports `extruder_max_nozzle_count=[1,1]`).
5. New **`bambu-studio-console.exe`** target (same program, console subsystem) to drive slicing from scripts.
6. Windows build with Visual Studio *Build Tools* only and without `pkg-config`.
7. **Light core** (November 2026), **off by default**. With the env var `RELIEVO_CORE_MARGIN_MM` (mm), sparse grid infill keeps
   its density within that distance of any surface (top, bottom, sides, cavities); deeper inside, only one grid line out of
   `RELIEVO_CORE_KEEP` (default 3) is printed. Kept lines cross the core uncut. `RELIEVO_CORE_LINK=1` also joins the cut ends
   along the core boundary. GitHub Actions build: `.github/workflows/relievo_win.yml`.
8. **Flush into infill without a prime tower** (November 2026), **off by default**. Upstream honours *Flush into objects'
   infill* only while planning the prime tower; with the tower disabled the whole purge goes to the chute. With the env var
   `RELIEVO_FLUSH_INFILL_NO_TOWER=1` and `flush_into_infill` on the object, each filament change marks internal infill of the
   layer to be printed with the new filament right after the change, and the chute purge (`flush_length`) is reduced by the
   absorbed volume, never below `RELIEVO_FLUSH_MIN_MM3` (default 45 mm3, nozzle pressure).
9. **Slicing that never ends** (November 2026). Repairing a degenerate Voronoi diagram rotates the input segments and rounds
   their endpoints, which can make two segments cross; `boost::polygon::construct_voronoi` then never returns (seen on painted
   multi-material layers: console stuck on one core). The rotated input is now checked and an angle that makes it invalid is
   skipped. On a plate that stalled 6 times out of 6, 16 runs out of 18 finish, with identical results.
10. **Painted regions split cleanly** (November 2026). Painted lines projected on a layer contour could overlap or point
   backwards, and the split contour then went back on itself, which the Voronoi computation does not support: degenerate
   diagram, whole blocks of a top layer coloured at random from one run to the next, sometimes slicing that never ends.
   Each contour segment is now rebuilt in order, from its start to its end. Test plate: invalid inputs 49 -> 4, failed
   repairs 47 -> 2, same filament per colour between runs. `RELIEVO_MM_NO_MONOTONIC=1` turns it off (comparisons).

Portable binary (Releases): unzip and run `bambu-studio.exe` / `bambu-studio-console.exe`. `resources` is a full copy; the
Visual C++ 2015-2022 x64 runtime is bundled (Microsoft redistributable DLLs); Windows 10 also needs the WebView2 runtime.
