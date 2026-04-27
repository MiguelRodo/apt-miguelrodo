# apt-miguelrodo

Personal APT repository hosted on GitHub Pages, providing Debian packages for various utilities.

## Add the repository

```bash
# 1. Install prerequisites
sudo apt-get install -y curl gpg

# 2. Add the GPG signing key
curl -fsSL https://miguelrodo.github.io/apt-miguelrodo/KEY.gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/apt-miguelrodo.gpg

# 3. Add the repository source
echo "deb [signed-by=/usr/share/keyrings/apt-miguelrodo.gpg] https://miguelrodo.github.io/apt-miguelrodo ./" \
  | sudo tee /etc/apt/sources.list.d/apt-miguelrodo.list

# 4. Update package lists
sudo apt-get update
```

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
