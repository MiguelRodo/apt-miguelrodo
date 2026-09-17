# apt-miguelrodo

Personal APT repository hosted on GitHub Pages, providing signed Debian packages for MiguelRodo utilities.

The repository uses a Debian-style multi-architecture layout (`pool/` + `dists/`) so `Architecture: all` packages and architecture-specific packages such as `amd64` and `arm64` can coexist in one source.

## Add the repository

```bash
# 1. Install prerequisites
sudo apt-get install -y curl gpg

# 2. Add the GPG signing key
curl -fsSL https://miguelrodo.github.io/apt-miguelrodo/KEY.gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/apt-miguelrodo.gpg

# 3. Add the repository source
echo "deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main" \
  | sudo tee /etc/apt/sources.list.d/apt-miguelrodo.list

# 4. Update package lists
sudo apt-get update
```

After that, install packages normally with `apt-get install <package>`.

## Repository layout

- Package files are stored under `pool/main/<bucket>/`, where the bucket is derived from the package name, for example `pool/main/p/` for `pj` and `pool/main/s/` for `setupmjr`.
- Package indexes are published per architecture under:
  - `dists/stable/main/binary-amd64/Packages.gz`
  - `dists/stable/main/binary-arm64/Packages.gz`
  - `dists/stable/main/binary-all/Packages.gz`
- Signed distribution metadata is published at:
  - `dists/stable/InRelease` (preferred)
  - `dists/stable/Release` + `dists/stable/Release.gpg`

APT should be treated as the installation interface. Consumers should not depend on a package's exact `pool/` path unless they genuinely need a direct `.deb` URL.

## Release-backed publishing

Packages are published from versioned upstream releases rather than by treating this repository as their source tree.

For `pj`, `.github/workflows/import-pj-release.yml` checks the latest `MiguelRodo/pj` GitHub Release, downloads that release's `pj_<version>_all.deb`, then imports the exact artifact into this repository and regenerates the signed APT metadata. This means the APT package follows released `pj` versions, not unreleased commits on `pj/main`.

Other release workflows may publish their generated Debian artifacts directly into this repository using the shared release tooling in `MiguelRodo/actions`.

## Available packages

The current repository indexes include these primary packages:

### `pj` — Local operator launcher

Source: [MiguelRodo/pj](https://github.com/MiguelRodo/pj)

```bash
sudo apt-get install -y pj
```

The package is architecture-independent and is imported from the versioned `pj` GitHub Release artifact.

### `setupmjr` — Cross-platform setup utility

Source: [MiguelRodo/setupmjr](https://github.com/MiguelRodo/setupmjr)

```bash
sudo apt-get install -y setupmjr
```

`setupmjr project --pj` installs or refreshes `pj` from its floating `v0` release line.

### `projects` — Deterministic GitHub Project administration CLI

Source: [MiguelRodo/github-projects-skill](https://github.com/MiguelRodo/github-projects-skill)

```bash
sudo apt-get install -y projects
```

### `repos` — Multi-repository management tool

Source: [MiguelRodo/repos](https://github.com/MiguelRodo/repos)

```bash
sudo apt-get install -y repos
```

Manage multiple Git repositories as a unified workspace from a `repos.list` file.

## Migration notes

### Old flat source entry

If you previously used:

```bash
deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo ./
```

replace it with:

```bash
deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main
```

### Architecture usage

- Shell and other architecture-independent packages may use `Architecture: all`.
- Compiled CLI packages may be published as `amd64`, `arm64`, or other architectures.
- A normal source entry is preferred. APT automatically considers the native architecture plus `all` packages.

To restrict an installation explicitly:

```bash
deb [arch=amd64 signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo stable main
```

### Direct `.deb` URLs

Old root URLs such as:

```text
https://miguelrodo.github.io/apt-miguelrodo/<package>_<version>_<arch>.deb
```

are not stable. Prefer APT. If a direct download is unavoidable, use the package's current path under:

```text
https://miguelrodo.github.io/apt-miguelrodo/pool/main/<bucket>/<package>_<version>_<arch>.deb
```
