# Release roadmap

The project is currently an in-progress recompilation (`0.1.0`). This roadmap tracks work needed before it can be described as a complete, user-buildable release. A checked box means the work has been implemented and verified; planned behavior below is not a claim about current release artifacts.

## Planned: Add supported target — owned-input build kits

**Status: Not started.** Model the public release flow on [NJH-1001/RevPRecompile](https://github.com/NJH-1001/RevPRecompile), especially its [README](https://github.com/NJH-1001/RevPRecompile#readme) and [`PUBLIC_RELEASE_POLICY.md`](https://github.com/NJH-1001/RevPRecompile/blob/main/PUBLIC_RELEASE_POLICY.md). Follow its separation between the small public setup/build kit and title code generated locally from user-supplied media.

### Desired release policy

> Public releases are deliberately owned-input build kits, not playable executables. They contain no game disc data, retail PlayStation BIOS, generated game code, generated retail-BIOS code, or prebuilt title executable. You supply your own matching disc dump and generate the executable locally.

This is the intended policy for future public release artifacts, not a description of the current release workflow. The existing workflow still builds and packages a prebuilt title executable.

### Define and document the supported target

Use a concise `Supported target` section like the reference project, and specify each of these rather than implying compatibility:

- **Initial host platform:** Windows 10/11, x86-64 (Intel or AMD) as the first owned-input kit target, following the reference project's initial platform scope. Treat Linux x86-64, macOS Intel x86-64, and macOS arm64 as follow-on kit targets until their complete first-run generation flows pass the same acceptance tests. This does not change the currently configured CI build matrix by itself.
- **Game identity:** Persona 2: Innocent Sin, Japanese PlayStation v1.0, serial `SLPS-02100`; the project's configured reference input is described in [`DISC.md`](DISC.md) and [`game.toml`](game.toml), including the cue filename's Gemini English translation v1.0a identity and input checks. Accept only the explicitly supported matching dump; do not imply a stock disc or a different revision/translation is compatible.
- **Input format and validation:** document the required cue/bin layout and matching checksums, how users obtain/select their own dump, and actionable errors for a missing or mismatched input. Never bundle or upload the input.
- **BIOS mode:** use the framework's permitted bundled OpenBIOS by default. Do not require, bundle, or generate from a retail BIOS for the supported default flow.

### Implementation work

- [ ] Define the target matrix and exact matching-disc contract; reconcile the instructions with `DISC.md`, `game.toml`, and the framework's setup wizard.
- [ ] Make the setup kit build emitters and generate game source from the user's selected disc in a local, ignored workspace, then compile the title executable on that user's machine. Keep generated game and retail-BIOS code and the locally built title executable out of every release archive.
- [ ] Pin and document the bootstrap toolchain (Python, CMake/Ninja or the supported bundled toolchain), supported compiler/toolchain versions, network needs, disk-space needs, and clean install/build steps.
- [ ] Add an explicit public release contents policy and automated archive checks. Reject disc images and extracted disc files, retail BIOS dumps, generated game code, generated retail-BIOS code, prebuilt title executables, saves/captures/analysis files, and private absolute paths. Include only the setup host, required emitters/framework/runtime components, configuration, and applicable license/attribution notices.
- [ ] Verify that the setup host never silently falls back to stale or checked-in generated game output when user input is absent, invalid, or mismatched. Keep generated outputs in ignored local paths and give clear failure/retry instructions.
- [ ] Test the complete flow from a clean checkout and a clean release-kit extraction: supply a matching user-owned disc, generate locally, compile, launch, and verify the resulting executable is local-only. Add negative tests for absent and mismatched input and inspect every produced archive against the denylist.
- [ ] Update the README's supported-target, setup, build, and release sections only after those tests pass; update `.recomp.json` and other status badges/metadata to reflect measured progress rather than planned work.

### Release and repository gates

- [ ] Review the licensing and redistribution status of original project code, generated game-derived output, framework dependencies, bundled OpenBIOS, and included assets. Keep the root GPL scope narrow and preserve each component's own notices.
- [ ] Decide and document how generated output is handled in the **public Git repository and source archives**, not just in release ZIPs. The desired release-kit exclusion alone does not remove the currently tracked `generated/` files from a public repository.
- [ ] Run the release archive audit and all supported-platform clean-machine tests on the exact commit to be tagged. Publish only artifacts that satisfy the policy above.

Until these tasks are complete, describe the project as in progress and do not describe the current prebuilt-executable release workflow as an owned-input build-kit workflow.
