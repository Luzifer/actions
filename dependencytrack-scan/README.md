# Dependency-Track Scan

Generate a CycloneDX 1.6 SBOM with Syft and upload it to Dependency-Track.

The action expects `syft` and `curl` to be available on the runner. It creates the Dependency-Track project version when it does not exist and uploads to the existing project version otherwise.

## Usage

```yaml
jobs:
  dependencytrack:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - uses: luzifer/actions/dependencytrack-scan@develop
        with:
          token: ${{ secrets.DEPENDENCYTRACK_TOKEN }}
          url: https://dependencytrack.example.com
```

By default this action checks out the repository and uses `<owner>/<repo>` as the project name. The version is the tag or branch name, or the source branch for pull requests. Tag builds mark newly created project versions as latest.

The API token requires `BOM_UPLOAD` and a project creation permission. `PROJECT_CREATION_UPLOAD` is the least-privilege option.

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `checkout` | `true` | Whether to run `actions/checkout` inside this action. |
| `latest` | `true` for tags, otherwise `false` | Whether a newly created project version is marked as latest. |
| `project` | `github.repository` | Dependency-Track project name. |
| `token` | required | Dependency-Track API token. |
| `url` | required | Dependency-Track instance URL without `/api/v1/bom`. |
| `version` | tag, branch, or pull-request source branch | Dependency-Track project version. |

## Examples

Use an existing checkout:

```yaml
- uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
  with:
    show-progress: false

- uses: luzifer/actions/dependencytrack-scan@develop
  with:
    checkout: 'false'
    token: ${{ secrets.DEPENDENCYTRACK_TOKEN }}
    url: https://dependencytrack.example.com
```

Override the project metadata:

```yaml
- uses: luzifer/actions/dependencytrack-scan@develop
  with:
    latest: 'true'
    project: Luzifer/twitch-bot
    token: ${{ secrets.DEPENDENCYTRACK_TOKEN }}
    url: https://dependencytrack.example.com
    version: v1.2.3
```
