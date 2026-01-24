# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Changed
- **2026-01-23**: Switched from `mesa-panfork-git` to mainline `mesa` in packages.aarch64
  - **Why**: The kernel now uses the panthor driver for Mali G610 GPU (CSF-based Valhall architecture)
  - **Previous**: `mesa-panfork-git` only includes `panfrost_dri.so` (for older Bifrost/Midgard GPUs)
  - **New**: Mainline `mesa` (24.1+) includes `panthor_dri.so` which is required for the panthor kernel driver
  - **Related**: Kernel patch `enable-panthor-gpu.patch` in pkgbuilds repo switches device tree from bifrost to panthor

### Fixed
- **2026-01-23**: Fixed inline comment syntax in packages.aarch64
  - **Issue**: Inline comments (`package  # comment`) not supported by imageforge parser
  - **Fix**: Moved comment to separate line above the package name
