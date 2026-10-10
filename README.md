# Bambu Studio — version patchée Relievo / Relievo patched build

> **Ce dépôt n'est pas le Bambu Studio officiel.** C'est la version que [Relievo](https://relievo.fr) utilise pour
> imprimer ses cartes en relief multicolores : Bambu Studio **v02.07.01.62** plus quelques modifications. Elle est
> publiée sous la même licence (**GNU AGPL v3**), comme celle-ci le demande. Ce n'est **pas affilié à Bambu Lab**.
> Le logiciel officiel se trouve sur [bambulab/BambuStudio](https://github.com/bambulab/BambuStudio).
>
> *This repository is **not** the official Bambu Studio. It is the build [Relievo](https://relievo.fr) uses to print
> multi-colour relief maps: Bambu Studio **v02.07.01.62** plus a few changes, published under the same **GNU AGPL v3**
> license, as it requires. **Not affiliated with Bambu Lab.** The official software is at
> [bambulab/BambuStudio](https://github.com/bambulab/BambuStudio).*

**Télécharger / Download (Windows 64 bits, portable) :**
[Releases → relievo-02.07.01.62](https://github.com/amilexx/BambuStudio-Relievo/releases/tag/relievo-02.07.01.62)

**Ce qui change / What changes** (détail complet / full details: [RELIEVO.md](RELIEVO.md)) :

| | |
|---|---|
| `color_penetration_taper` | Nouvelle option : la couleur peinte descend à pleine largeur au lieu de former un cône, pour que les routes et rivières fines gardent leur couleur. *New option: painted colour goes straight down instead of tapering.* |
| `mmu_segmented_region_max_width` | Défaut 2 mm et réglage réaffiché. *Default 2 mm, shown again in the UI.* |
| Îlots résiduels / residual islands | Petits restes entre volumes quasi coïncidents retirés. *Removed between near-coincident volumes.* |
| Prime Saving | Proposé sur toute machine multi-extrudeur (X2D). *Offered on any multi-extruder machine.* |
| `bambu-studio-console.exe` | Version ligne de commande pour trancher par script. *Console build for scripted slicing.* |

Toutes les modifications tiennent dans **un seul commit** par-dessus le tag officiel.
*All changes are in **one commit** on top of the official tag:*
[`v02.07.01.62...relievo/v02.07.01.62`](https://github.com/amilexx/BambuStudio-Relievo/compare/v02.07.01.62...relievo/v02.07.01.62).

---

*Ci-dessous : le README d'origine de Bambu Studio, inchangé. / Below: the original Bambu Studio README, unchanged.*

---

![image](https://user-images.githubusercontent.com/106916061/179006347-497d24c0-9bd6-45b7-8c49-d5cc8ecfe5d7.png)
# BambuStudio
Bambu Studio is a cutting-edge, feature-rich slicing software.  
It contains project-based workflows, systematically optimized slicing algorithms, and an easy-to-use graphic interface, bringing users an incredibly smooth printing experience.

Prebuilt Windows, macOS 64-bit and Linux releases are available through the [github releases page](https://github.com/bambulab/BambuStudio/releases/).

Bambu Studio is based on [PrusaSlicer](https://github.com/prusa3d/PrusaSlicer) by Prusa Research, which is from [Slic3r](https://github.com/Slic3r/Slic3r) by Alessandro Ranellucci and the RepRap community.

See the [wiki](https://github.com/bambulab/BambuStudio/wiki) and the [documentation directory](https://github.com/bambulab/BambuStudio/tree/master/doc) for more information.

# What are Bambu Studio's main features?
Key features are:
- Basic slicing features & GCode viewer
- Multiple plates management
- Remote control & monitoring
- Auto-arrange objects
- Auto-orient objects
- Hybrid/Tree/Normal support types, Customized support
- multi-material printing and rich painting tools
- multi-platform (Win/Mac/Linux) support
- Global/Object/Part level slicing parameters

Other major features are:
- Advanced cooling logic controlling fan speed and dynamic print speed
- Auto brim according to mechanical analysis
- Support arc path(G2/G3)
- Support STEP format
- Assembly & explosion view
- Flushing transition-filament into infill/object during filament change

# How to compile
Following platforms are currently supported to compile:
- Windows 64-bit, [Compile Guide](https://github.com/bambulab/BambuStudio/wiki/Windows-Compile-Guide)
- Mac 64-bit, [Compile Guide](https://github.com/bambulab/BambuStudio/wiki/Mac-Compile-Guide)
- Linux, [Compile Guide](https://github.com/bambulab/BambuStudio/wiki/Linux-Compile-Guide)
  - currently we only provide linux appimages on [github releases](https://github.com/bambulab/BambuStudio/releases) for Ubuntu/Fedora, and a [flathub version](https://flathub.org/apps/com.bambulab.BambuStudio) can be used for all the linux platforms

# Report issue
You can add an issue to the [github tracker](https://github.com/bambulab/BambuStudio/issues) if **it isn't already present.**

# License
Bambu Studio is licensed under the GNU Affero General Public License, version 3. Bambu Studio is based on PrusaSlicer by PrusaResearch.

PrusaSlicer is licensed under the GNU Affero General Public License, version 3. PrusaSlicer is owned by Prusa Research. PrusaSlicer is originally based on Slic3r by Alessandro Ranellucci.

Slic3r is licensed under the GNU Affero General Public License, version 3. Slic3r was created by Alessandro Ranellucci with the help of many other contributors.

The GNU Affero General Public License, version 3 ensures that if you use any part of this software in any way (even behind a web server), your software must be released under the same license.

The bambu networking plugin is based on non-free libraries. It is optional to the Bambu Studio and provides extended networking functionalities for users.
By default, after installing Bambu Studio without the networking plugin, you can initiate printing through the SD card after slicing is completed.

