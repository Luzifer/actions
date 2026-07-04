# Docker Publish

Build and optionally publish a Docker image to a registry.

## Usage

```yaml
jobs:
  docker:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: luzifer/actions/docker-publish@develop
```

By default this action checks out the repository, builds the image from the repository root, and pushes on non-pull-request events. The default image name is `ghcr.io/<owner>/<repo>` in lowercase.

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `build-secrets` | empty | Newline-separated BuildKit secrets passed to `docker/build-push-action`. |
| `context` | `.` | Docker build context. |
| `checkout` | `true` | Whether to run `actions/checkout` inside this action. |
| `image` | `<registry>/<owner>/<repo>` | Image name including registry. |
| `password` | `github.token` | Registry password or token. |
| `push` | empty | Set to `true` or `false`. When empty, pushes unless the event is `pull_request`. |
| `registry` | `ghcr.io` | Docker registry hostname. |
| `username` | `github.actor` | Registry username. |

## Examples

Build without pushing:

```yaml
- uses: luzifer/actions/docker-publish@develop
  with:
    push: 'false'
```

Publish a custom image:

```yaml
- uses: luzifer/actions/docker-publish@develop
  with:
    image: ghcr.io/example/project-api
```

Pass BuildKit secrets:

```yaml
- uses: luzifer/actions/docker-publish@develop
  with:
    build-secrets: |
      GITHUB_COM_TOKEN=${{ secrets.GH_COM_TOKEN }}
```

Dockerfile:

```dockerfile
RUN --mount=type=secret,id=GITHUB_COM_TOKEN \
    GITHUB_COM_TOKEN="$(cat /run/secrets/GITHUB_COM_TOKEN)" ./build.sh
```
