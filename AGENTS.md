# Repository Guidelines

## Project Structure & Module Organization

Virtual AGC combines historical source transcriptions with emulator and tooling code. Mission software lives in top-level directories such as `Luminary099/`, `Colossus249/`, `Comanche072/`, `Sunburst120/`, and `Validation/`, usually centered on `MAIN.agc`. Emulator and utility programs live in `yaAGC/`, `yaAGS/`, `yaASM/`, `yaYUL/`, `yaLEMAP/`, `yaLVDC/`, `yaOBC/`, `yaUniverse/`, and `Tools/`. GUI/peripheral tools are under `yaDSKY2/`, `yaDEDA2/`, `yaPanel/`, `yaTelemetry/`, and `VirtualAGC/`. Documentation and deployment material are in `README.md`, `doc/`, `agcSoftwareDocumentation/`, and `Docker/`.

## Build, Test, and Development Commands

- `cmake --workflow default`: configures `build/`, builds the CMake-supported tools, and runs CTest. This is the CI path.
- `cmake --workflow release`: release-configured build and test workflow.
- `ctest --preset default`: reruns the registered tests after a CMake build.
- `make`: legacy recursive build for the broader Virtual AGC tree on Linux-like systems.
- `make missions`: assembles mission directories listed by the top-level Makefile.
- `make -C yaASM` or `make -C Luminary099`: builds one component or mission.
- `make clean`: removes build outputs.
- `cd Docker && docker-compose up -d`: runs the packaged desktop/VNC environment.

## Coding Style & Naming Conventions

Follow the surrounding file style and avoid mass reformatting historical sources. CMake currently builds C as C99 and C++ as C++17; use portable POSIX-friendly C/C++ unless a module documents otherwise. Prefer `snake_case` for new C helpers, `UPPER_SNAKE_CASE` for macros, and existing AGC label conventions inside `.agc` transcriptions. Check system-call and allocation failures. Commit generated artifacts only when they are intentional reference outputs.

## Testing Guidelines

CTest currently registers focused tests for `yaASM` and `yaUniverse`; add new CMake tests with `add_test()` near the target being exercised. For assembler or mission changes, run the relevant `make -C <mission>` target and compare regenerated `.lst`, `.bin`, or `corediff.txt` outputs when applicable. Keep tests deterministic and short.

## Commit & Pull Request Guidelines

Recent history uses concise descriptive subjects such as `Added extractSTS83.py.` or `More tweaks for extractSTS83.py.` rather than strict conventional-commit prefixes. Name the affected tool, mission, or data source in the first line. Pull requests should summarize behavior changes, list build/test commands run, mention intentional generated-file updates, link related issues, and include screenshots only for GUI changes.

## Security & Configuration Tips

Do not commit local paths, credentials, VM state, or build logs. Docker exposes noVNC/VNC on ports `6080` and `5900`; keep those local unless you intentionally publish the service.
