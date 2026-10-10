# Bambu Studio — version patchée Relievo

**Base :** Bambu Studio `v02.07.01.62` (tag officiel de [bambulab/BambuStudio](https://github.com/bambulab/BambuStudio)).
**Modifications :** Relievo (relievo.fr), de juillet à septembre 2026. Cette branche contient le tag d'origine plus **un seul
commit**, qui regroupe toutes les modifications. `git diff v02.07.01.62` les montre en entier (10 fichiers, environ 100 lignes).
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

## Compiler

Comme l'original (voir `doc/` et le README d'origine) : dépendances avec `build_win.bat -d <dossier_deps> -s deps`, puis
l'application avec `build_win.bat -d <dossier_deps> -s app`. Le binaire publié a été compilé en `RelWithDebInfo` (MSVC 2022,
Windows 11, 18/09/2026).

## Binaire portable (Releases)

Zip à décompresser, puis lancer `bambu-studio.exe` (interface) ou `bambu-studio-console.exe` (ligne de commande).
- Le dossier `resources` est une **copie complète**. Dans l'arbre de compilation, c'est une jonction vers les sources.
- Le runtime **Visual C++ 2015-2022 (x64)** est joint (DLL redistribuables de Microsoft). Sous Windows 10, l'interface demande
  aussi le runtime **WebView2**, présent d'office sous Windows 11.
- Les profils et réglages sont partagés avec un Bambu Studio officiel installé (`%APPDATA%\BambuStudio`).

---

# Bambu Studio — Relievo patched build (English)

**Base:** Bambu Studio `v02.07.01.62` (official tag of [bambulab/BambuStudio](https://github.com/bambulab/BambuStudio)).
**Modified by:** Relievo (relievo.fr), July–September 2026. This branch is the original tag plus **one commit** holding every
change; `git diff v02.07.01.62` shows all of them (10 files, ~100 lines).
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

Portable binary (Releases): unzip and run `bambu-studio.exe` / `bambu-studio-console.exe`. `resources` is a full copy; the
Visual C++ 2015-2022 x64 runtime is bundled (Microsoft redistributable DLLs); Windows 10 also needs the WebView2 runtime.
