# Licensing scope

This file clarifies the intended scope of the root [`LICENSE`](LICENSE). It is not legal advice and does not change any third party's rights or license terms.

## Original project code and documentation

The project intends to license its original, project-authored code and documentation under **GNU GPL version 3 only (SPDX: `GPL-3.0-only`)**, to the extent the project has the rights to do so. The root `LICENSE` contains the unmodified text of that license. This grant does not apply to files identified below, files carrying their own license or attribution, or third-party material.

## Explicit exclusions and separate terms

- **`generated/**`: excluded from the root GPL grant.** These generated files are derived from analysis of a user-supplied Persona 2: Innocent Sin disc, identified in [`DISC.md`](DISC.md). They may contain game-derived material. The root `LICENSE` grants no rights to the game, any translation, or these generated outputs. Their presence here is not a statement that redistribution is authorized. See [`GENERATED-CODE-NOTICE.md`](GENERATED-CODE-NOTICE.md) and the notice inside [`generated/README.md`](generated/README.md).
- **Framework submodules and their dependencies:** `psxrecomp/`, `recomp-ui/`, and nested dependencies retain their own licenses and notices. For example, `psxrecomp/LICENSE` is PolyForm Noncommercial 1.0.0 and `recomp-ui/LICENSE` is MIT. Review the terms that apply to each component; the root GPL does not replace them.
- **`assets/psxrecomp.*`:** these icons are sourced from PSXRecomp's `assets/` directory and are not part of the project's original-code GPL grant. Consult the applicable upstream terms.
- **Other third-party or game-derived material:** any separately identified material remains under its own applicable terms and is not relicensed by the root `LICENSE`.

No license in this repository grants permission to obtain, use, reproduce, or distribute the original game or translation. A locally supplied disc and local regeneration do not, by themselves, establish permission to redistribute generated source or binaries. Do not add disc images, raw disc data, BIOS dumps, or game assets to this repository; see [`.gitignore`](.gitignore) and [`DISC.md`](DISC.md).
