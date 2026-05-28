## Cursor Cloud specific instructions

- This repository builds UN1CA custom Samsung firmware ZIPs with shell scripts; there are no long-running web, API, database, or worker services to start. Treat the build scripts themselves as the runnable service surface.
- The CI build flow in `.github/workflows/build.yml` is the source of truth for the standard local sequence: source `./buildenv.sh <target>`, then run `./scripts/build_dependencies.sh`, `./scripts/download_fw.sh`, `./scripts/extract_fw.sh`, and `./scripts/make_rom.sh`.
- Use one of the targets under `target/` when sourcing `buildenv.sh`; `a52sxq` is a quick default for smoke checks because it exercises the same shared tooling path as the matrix targets.
- `scripts/build_dependencies.sh` expects external git submodules to be initialized first. The Cursor Cloud startup update script handles that with `git submodule update --init --recursive`.
- Direct `samloader` smoke checks require activating `out/tools/venv` after `build_dependencies.sh` has run; the repository's `download_fw.sh` script activates this virtualenv automatically.
- Full firmware extraction and ROM builds can require large Samsung firmware downloads plus privileged image mount support for EROFS/F2FS partitions. Lightweight environment checks can validate the same external firmware path with `samloader ... checkupdate` before starting multi-gigabyte downloads.
