# apt-miguelrodo

Personal APT repository hosted on GitHub Pages, providing Debian packages for various utilities.

This repository now uses a Debian-style multi-architecture layout (`pool/` + `dists/`) to support both legacy `all` packages and architecture-specific packages (`amd64`, `arm64`, etc.) in one APT repository. See [MiguelRodo/actions#26](https://github.com/MiguelRodo/actions/issues/26).

## Add the repository (new layout)

```bash
# 1. Install prerequisites
sudo apt-get install -y curl gpg

# 2. Add the GPG signing key
curl -fsSL https://miguelrodo.github.io/apt-miguelrodo/KEY.gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/apt-miguelrodo.gpg

# 3. Add the repository source (distribution/component layout)
echo "deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main" \
  | sudo tee /etc/apt/sources.list.d/apt-miguelrodo.list

# 4. Update package lists
sudo apt-get update
```

## Repository layout

- Package files (`.deb`) are published under `pool/main/r/`
- Package indexes are published per architecture under:
  - `dists/stable/main/binary-amd64/Packages.gz`
  - `dists/stable/main/binary-arm64/Packages.gz`
  - `dists/stable/main/binary-all/Packages.gz`
- Distribution metadata is published at `dists/stable/Release`

## Migration notes

### If you used the old flat source entry

Old source line:

```bash
deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo ./
```

Replace it with:

```bash
deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main
```

### Architecture usage

- Legacy shell-script packages are `Architecture: all`
- Newer binaries (for example Go CLI packages) may be published as `amd64`, `arm64`, etc.
- Keep using normal `apt-get install <package>`; APT resolves the correct package for your architecture while also considering `all` packages.

Examples:

```bash
# Standard (native architecture + all)
deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main

# Restrict to one architecture explicitly
deb [arch=amd64 signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main

# Multi-arch client example
deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main
```

### If you hardcoded direct `.deb` URLs

Direct root URLs such as:

- `https://miguelrodo.github.io/apt-miguelrodo/<package>_<version>_<arch>.deb`

are no longer stable.

Use one of these instead:

1. Preferred: install via APT (`apt-get install`) after adding the source.
2. If direct download is required, use the new `pool/` path:
   - `https://miguelrodo.github.io/apt-miguelrodo/pool/main/r/<package>_<version>_<arch>.deb`

## Available packages

### `repos` — Multi-Repository Management Tool

Source: [MiguelRodo/repos](https://github.com/MiguelRodo/repos)

Manage multiple Git repositories as a unified workspace from a single `repos.list` file.

```bash
sudo apt-get install -y repos
```

**What it does:**

- Clone and manage multiple Git repositories defined in a `repos.list` file
- Create and manage Git worktrees across repositories
- Generate VS Code workspace configurations
- Run scripts/pipelines across all repositories

**Quick start:**

```bash
# Create repos.list listing the repositories you want
echo "myorg/data-curation
myorg/analysis
myorg/documentation" > repos.list

# Clone all listed repositories
repos clone
```

See the [full documentation](https://miguelrodo.github.io/repos) for more details.

### `infra` — Infrastructure Utilities

Source: [MiguelRodo/infra](https://github.com/MiguelRodo/infra)

```bash
sudo apt-get install -y infra
```
