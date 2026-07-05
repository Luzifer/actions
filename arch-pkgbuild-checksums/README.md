# Arch PKGBUILD Checksums

Update checksums in an Arch Linux `PKGBUILD`, reset `pkgrel` to `1`, and commit the result.

The action expects to run in an environment where `updpkgsums-builder` is available, for example `ghcr.io/luzifer-docker/gh-arch-env:latest`.

## Usage

```yaml
jobs:
  pkgbuild:
    runs-on: ubuntu-latest

    container:
      image: ghcr.io/luzifer-docker/gh-arch-env:latest

    permissions:
      contents: write
      
    steps:
      - uses: luzifer/actions/arch-pkgbuild-checksums@develop
```

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `checkout` | `true` | Whether to run `actions/checkout` inside this action. |
| `commit-author` | `Luzifer-CI <ci@luzifer.io>` | Commit author used by `git-auto-commit-action`. |
| `commit-message` | `Update PKGBUILD checksums` | Commit message for the checksum update. |
| `commit-user-email` | `ci@luzifer.io` | Commit user email used by `git-auto-commit-action`. |
| `commit-user-name` | `Luzifer-CI` | Commit user name used by `git-auto-commit-action`. |

## Examples

Use an existing checkout:

```yaml
- uses: actions/checkout@9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0 # v7.0.0
  with:
    show-progress: false

- uses: luzifer/actions/arch-pkgbuild-checksums@develop
  with:
    checkout: 'false'
```
