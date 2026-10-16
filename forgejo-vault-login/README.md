# Forgejo Vault Login

Authenticate the Vault CLI using an OpenID Connect token issued by Forgejo Actions.

The action expects `curl`, `jq`, and `vault` to be available on the runner. It exchanges the Forgejo JWT at the configured Vault JWT backend and exports `VAULT_ADDR` and `VAULT_TOKEN` for subsequent steps in the same job.

## Usage

```yaml
jobs:
  deploy:
    runs-on: forgejo
    enable-openid-connect: true
    env:
      VAULT_ADDR: https://vault.example.com
    steps:
      - uses: luzifer/actions/forgejo-vault-login@develop

      - run: vault kv get secret/example
```

OpenID Connect must be enabled at the workflow or job level. Without it, Forgejo does not provide the request URL and token required by this action.

By default, the action logs into the `forgejo-jwt` authentication backend with the role `forgejo-<repository name>`. For example, `luzifer/dockerhost` uses the role `forgejo-dockerhost`. When no audience is specified, Forgejo uses its default audience.

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `audience` | Forgejo default | Audience for the OIDC token. |
| `backend` | `forgejo-jwt` | Vault JWT authentication backend mount name below `auth/`. |
| `role-name` | `forgejo-<repository name>` | Vault role name. |
| `vault-addr` | `${{ env.VAULT_ADDR }}` | Vault server address. The effective value is exported as `VAULT_ADDR` for subsequent steps. |

## Examples

Use a custom audience:

```yaml
- uses: luzifer/actions/forgejo-vault-login@develop
  with:
    audience: https://vault.example.com
```

Override the authentication backend and role:

```yaml
- uses: luzifer/actions/forgejo-vault-login@develop
  with:
    backend: ci-jwt
    role-name: deploy-production
    vault-addr: https://vault.example.com

- run: vault read secret/example
```
