# Persona 2 Innocent Sin Recompiled

<!-- retcomm-readme-metrics -->
[![GitHub downloads (all assets, all releases)](https://img.shields.io/github/downloads/jermspeaks/Persona2Recomp/total)](https://github.com/jermspeaks/Persona2Recomp/releases)
[![GitHub downloads (latest release)](https://img.shields.io/github/downloads/jermspeaks/Persona2Recomp/latest/total)](https://github.com/jermspeaks/Persona2Recomp/releases/latest)
[![GitHub release](https://img.shields.io/github/v/release/jermspeaks/Persona2Recomp)](https://github.com/jermspeaks/Persona2Recomp/releases/latest)
<!-- /retcomm-readme-metrics -->

An in-progress static recompilation of **Persona 2: Innocent Sin** for PlayStation, built with [PSXRecomp](https://github.com/mstan/psxrecomp) and [recomp-ui](https://github.com/RetroPortingToolKit/recomp-ui). The project is currently at source version **0.1.0**; see [`VERSION`](VERSION) and [`.recomp.json`](.recomp.json) for the machine-readable release and development status.

| | |
|---|---|
| Original system | PlayStation (PS1) |
| Players | 1 |
| Original region / serial | Japan / `SLPS-02100` |
| Publisher | Atlus |
| Reference disc | Japanese v1.0; the recorded cue filename identifies the Gemini English translation v1.0a |

The reference image is identified by the disc checksums in [`game.toml`](game.toml), not by a file included here. See [`DISC.md`](DISC.md) for the exact expected input and user-supplied-disc requirements.

<!-- retcomm-readme-launcher -->
## Retro Launcher

You can run this title **standalone** (download the release zip, point it at
your disc, play), or manage installs, updates, and disc/BIOS wiring with
**[Retro Launcher](https://github.com/jermspeaks/Retro-Launcher)** —
the Retro Compilation Manager hub for self-compiling recomps.

[Downloads](https://github.com/jermspeaks/Retro-Launcher/releases) ·
[Full README & features](https://github.com/jermspeaks/Retro-Launcher#readme)

<p align="center">
  <img src="https://raw.githubusercontent.com/jermspeaks/Retro-Launcher/main/docs/screenshots/hub-and-game-launcher.png" alt="Retro hub with a background build, next to a title’s recomp-ui launcher" width="720">
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/jermspeaks/Retro-Launcher/main/docs/screenshots/queue-and-background-build.png" alt="Background cmake build with titles queued" width="720">
</p>

Retro checks for updates, installs the prebuilt release zips, and automates
BIOS/ROM/save plumbing so you are not stuck repeating each game’s first run by hand.
<!-- /retcomm-readme-launcher -->

## Legal and source contents

**No disc image, raw disc sectors, retail BIOS dump, or memory-card save is included in this repository, and none should ever be added.** The ignore rules in [`.gitignore`](.gitignore) exclude the local `disc/`, `bios/`, `saves/`, common image formats, and analysis scratch directories. Keep your own legally obtained disc and any optional BIOS dump on your machine; do not commit or upload them.

For clarity, this repository is **not just configuration and notes**: it commits generated C under [`generated/`](generated/). That source is produced by static recompilation from a developer-supplied disc and is compiled into the application. It is source code, not a raw disc image, and its presence has separate copyright/licensing implications. The framework-generated OpenBIOS backend is also included through the pinned `psxrecomp` submodule; release packages use the framework's bundled OpenBIOS rather than redistributing a retail BIOS dump.

The root [`LICENSE`](LICENSE) contains the **GNU General Public License, version 3 (GPL-3.0)**. It states the project's chosen license for files the project has the right to license; it does not grant rights to Atlus's game, the translation, or other third-party material. In particular, the checked-in [`generated/`](generated/) C was derived by recompiling a user-supplied commercial game disc. Adding a project license does not settle whether that game-derived source may lawfully be redistributed. Review those rights before distributing source or binaries, and do not treat this repository's license as permission to use the original game.

Dependencies retain their own terms: `psxrecomp/LICENSE` is PolyForm Noncommercial 1.0.0, and `recomp-ui/LICENSE` is MIT. The dependency licenses govern their respective components and are not replaced by the root license. The noncommercial terms on PSXRecomp may affect how a combined build can be used or distributed; review the dependency notices and terms. Release packaging includes third-party notices, which do not grant rights to game-derived source.

This is an independent fan project and is not affiliated with or endorsed by Atlus or the original game's other rights holders. This repository does not provide the game, a disc image, a translation patch, or a retail BIOS. Users are responsible for obtaining compatible source media lawfully and complying with applicable law. See [`DISC.md`](DISC.md).

`generated/` is intentionally tracked because release builds compile the checked-in output; regenerate it when seeds, code-generation inputs, or the framework pin change. Do not add raw disc/BIOS files while doing so.

Default app icon: `assets/psxrecomp.ico` (and `.png` / `.svg`) — Retro-themed controller mark from `psxrecomp/assets/`. Windows builds embed it via `APP_ICON`.

Optional box art under `launcher_assets/img/` may come from
[libretro-thumbnails](https://github.com/libretro-thumbnails/libretro-thumbnails)
(`Named_Boxarts`); see `BOXART_SOURCE.txt` when present.

## Build it yourself

### Requirements

You need Git (including submodule support), CMake 3.20+, Ninja, Python 3, and a native C/C++ compiler. Install them with the option for your host:

**Windows (MSVC):** install Git for Windows, Python 3, CMake, and Ninja (for example with `winget install Git.Git Python.Python.3.12 Kitware.CMake Ninja-build.Ninja`), plus Visual Studio 2022 Build Tools with the **Desktop development with C++** workload. Run CMake from a Developer PowerShell. Git Bash is needed only for the optional code-generation script.

**Windows (MSYS2/MinGW-w64):** in an MSYS2 MINGW64 shell, run `pacman -S --needed git python mingw-w64-x86_64-toolchain mingw-w64-x86_64-cmake mingw-w64-x86_64-ninja`. Git for Windows is an alternative source for the Bash environment.

**macOS:** install Xcode Command Line Tools with `xcode-select --install`, then run `brew install git cmake ninja python`.

**Linux (Debian/Ubuntu):** run `sudo apt install git build-essential cmake ninja-build python3`.

CMake fetches pinned SDL3/zlib sources when suitable system packages are not available, so the first configure may need network access. The checked-in generated game C means an ordinary build does **not** need your disc or the recompiler emitters.

### Compile the checked-in game source

```bash
git clone --recurse-submodules https://github.com/jermspeaks/Persona2Recomp.git
cd Persona2Recomp
cmake -S . -B build-release -G Ninja \
  -DCMAKE_BUILD_TYPE=Release \
  -DPSXRECOMP_REQUIRE_GAME_C=ON
cmake --build build-release --target psx-runtime
```

The executable is written under `build-release/` (or a configuration-specific subdirectory chosen by your CMake generator). The project enables the first-run setup wizard; running the build still requires you to select your own compatible disc image. The framework's bundled OpenBIOS is used by default, so a retail BIOS dump is not required.

### Regenerate the recompiled C (optional)

Only do this if you are changing seeds/code-generation inputs, updating the framework, or starting from a checkout without the committed output. Obtain the exact compatible disc input described in [`DISC.md`](DISC.md) yourself and keep it in the ignored `disc/` directory. Then build the host-side emitters and generate the source:

```bash
bash psxrecomp/tools/ci/build_emitters.sh
python3 psxrecomp/psxrecomp_cli.py generate \
  --config game.toml --project-root . --disc disc/<your-compatible-disc>.cue
```

Rebuild with the CMake commands above. If you are contributing a regeneration, review the diff and ensure that no disc image, extracted asset, BIOS dump, or save has entered the change. The generated C is tracked by design; raw media is not.

## Build targets and releases

The release workflow is configured to build these 64-bit targets:

| Artifact | Platform |
|---|---|
| `pis-<version>-linux-x64.zip` | Linux x86-64 |
| `pis-<version>-windows-x64.zip` | Windows x86-64 |
| `pis-<version>-macos-x64.zip` | macOS Intel x86-64 |
| `pis-<version>-macos-arm64.zip` | macOS Apple Silicon arm64 |

These are configured build targets; a target listing is not a claim that a binary has already been published. The GitHub Actions workflow at [`.github/workflows/release.yml`](.github/workflows/release.yml) builds the tracked generated C, packages the compiled game, and checks the package for disc/BIOS files. To prepare the first release, run **Actions → Persona 2 Innocent Sin release builds** with version `0.1.0` and publishing disabled first. A successful release attaches one ZIP per target. Release ZIPs are prebuilt applications, not source kits; bring your own compatible disc to play. The workflow requires a root `LICENSE` when publishing, but that check is not a legal review.

The repository's current semantic version is `0.1.0` in [`VERSION`](VERSION); the intended first tag is `v0.1.0`. Editing the version file does not create a git tag or hosted release. **Before publishing or distributing source, review the rights and licensing compatibility of generated game-derived C and the separately licensed dependencies.** The dispatch workflow defaults to building artifacts without publishing to avoid an accidental release.

## Symbols

Progressive map: `symbols.toml` → `python3 tools/sync_symbols.py` →
`psx_symbols.h` (`PSX_FN_*`). See `psxrecomp/docs/SYMBOLS.md`.

## Source and framework versions

The target source is the Japanese PlayStation release **Persona 2: Innocent Sin v1.0**, serial `SLPS-02100`. The reference cue filename recorded in `game.toml` describes an image with the **Gemini English translation v1.0a** applied. This is a disc-image identity, not media supplied by this repository; see [`DISC.md`](DISC.md) and the expected Track 01 digests in [`game.toml`](game.toml).

The build is pinned to these exact submodule revisions (the gitlinks are authoritative; `framework_pins.txt` is a human-readable snapshot):

| Component | Version / revision |
|---|---|
| PSXRecomp | `v0.3.2-alpha-1445-g2a4824fc` · `2a4824fcb897cb635438958f0c8cffb5a27df656` |
| recomp-ui | `41c88ffea852b970af8d50312620b17404e455ad` |
| recomp-net | `v0.1.9` · `e53059306824edf4f0aec4aae5ae65151b18535f` |
| retcomm-rbengine | `2a03e73693acee0fb78076ea058642c931bef12e` |

The runtime uses SDL3 by default (the pinned upstream release is SDL 3.4.10 when fetched by the framework); see the PSXRecomp submodule's build documentation for toolchain and dependency details. Keep framework submodules pinned to reviewed commits rather than updating to a moving branch during a release.

<!-- retcomm-readme-raid -->
---

<p align="center">
  <sub><b>R.A.I.D. — Retro AI Development</b> · a Discord for AI-assisted retro reverse-engineering, decomp &amp; recomp</sub>
</p>

<p align="center">
  <a href="https://discord.gg/Ad9BwSzctP"><img src=".github/raid-discord.png" alt="Join the Retro AI Development (R.A.I.D.) Discord" width="200"></a>
</p>
<!-- /retcomm-readme-raid -->
