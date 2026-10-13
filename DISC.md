# Disc requirement and compatibility

This project does not include a PlayStation disc image, extracted disc files, or a translation patch. The game must be supplied by the user from media they are legally entitled to use. Never place a disc image, extracted files, or patch payload in this repository or a release upload; the ignore rules in [`.gitignore`](.gitignore) are intended to keep local media out of Git.

## Reference version

The project configuration identifies the original game as **Persona 2: Innocent Sin**, Japanese PlayStation release **v1.0**, serial **SLPS-02100**. The cue filename recorded during local setup identifies the analyzed image as having the **Gemini English translation v1.0a** applied. This repository contains neither that disc image nor the patch. The currently checked-in generated game code and runtime configuration are based on that reference identity; a different language, revision, or patch may not be compatible.

The reference configuration currently expects a one-track cue/disc layout and checks the data track identity in [`game.toml`](game.toml):

- Track 01 size: `691925472` bytes
- Track 01 MD5: `11b188975894f2e4062ac773a9e9c153`
- Track 01 SHA-1: `cd3521fab99346ada2f09391b1c415429fde2b1c`
- Track 01 CRC-32: `870ceff9`

These values identify the project's configured input; they are not a download source or proof of rights to use the game or translation. A stock disc or another translation/revision will not match unless the project configuration and generated output are deliberately updated for it.

## Running the built game

Use the first-run setup wizard to select your own compatible cue/bin files. Release packages are built with the framework's bundled OpenBIOS; a retail BIOS dump is not included and is not required for the default setup. Do not share or upload your disc or BIOS files when asking for support.

This is an independent fan project, not affiliated with or endorsed by Atlus or other rights holders. The project does not grant rights to the original game, its assets, or any translation. Check the laws and rights that apply where you live before obtaining, modifying, or using game media.