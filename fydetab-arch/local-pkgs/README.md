# Local Package Cache

This directory holds locally-built packages that should be used instead of remote repository versions.

## Usage

1. Copy `.pkg.tar.zst` files to this directory
2. Generate repository database:
   ```bash
   repo-add local.db.tar.gz *.pkg.tar.zst
   ```
3. Build image normally - pacman will check here first

## Automated

The `build-image.sh` script handles this automatically:
1. Copies packages from `pkgbuilds/` subdirectories
2. Generates the repository database
3. Runs ImageForge

## Notes

- Packages here take priority over Fyde and Arch repos
- Remove old packages when updating to avoid conflicts
- The `local.db.tar.gz` must be regenerated after adding packages
