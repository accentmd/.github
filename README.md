# .github

Shared defaults for every repository in the `accentmd` organization.

This repository is public because GitHub applies default community files only from a public `.github` repository. It must never contain hostnames, IP addresses, secrets or environment-specific details.

| Path | What |
|---|---|
| `profile/README.md` | The organization profile |
| `.github/ISSUE_TEMPLATE/` | Default issue forms (below) |
| `.github/pull_request_template.md` | Default pull request template |
| `.github/workflows/deploy.yml` | Reusable: deploy a release to an environment |
| `.github/workflows/rollback.yml` | Reusable: switch a service back to its previous release |
| `.github/workflows/secret-scan.yml` | Reusable: gitleaks over the full history |
| `.github/workflows/self-ci.yml` | This repository's own checks (actionlint, zizmor, yamllint), on GitHub-hosted runners only |
| `workflow-templates/` | Starter workflows offered in every repository's Actions tab: service CI and service deploy |
| `renovate/default.json` | Shared Renovate preset: `"extends": ["github>accentmd/.github//renovate/default"]` |

A repository's own file of the same type replaces the default. For example, `web` has its own issue templates.

## Issue templates

| Template | When | Issue type | Label |
|---|---|---|---|
| `epic.yml` | A large piece of work split into tasks (sub-issues) | Epic | — |
| `task.yml` | One pass, one pull request | Task | `type: development` |
| `data-gap.yml` | Data is unavailable or unreliable | Task | `type: data` |
| `external-request.yml` | Something is needed from someone outside the team | External request | — |

Blocking is expressed with GitHub's native "blocked by" links, and hierarchy with sub-issues. Work is tracked in the [Accent Roadmap](https://github.com/orgs/accentmd/projects) project.

## Deploy and rollback

`deploy.yml` deploys one published release of one service to one environment.

1. **Gate.** The run must be started by `workflow_dispatch` (unless `manual-only: false`). The actor must be listed in the environment's `DEPLOY_ACTORS`. The tag must be a published, non-draft release.
2. **Resolve and verify the images.** For every image in the release's `deploy/deploy.yml`, it resolves `<repository>:<version>` to a digest. It then checks with cosign that the digest was signed by the caller's release workflow at exactly that tag.
3. **Decrypt the config.** It decrypts `deploy/env/<environment>.<name>.sops.env` with the environment's age key into a tmpfs directory.
4. **Ship.** It streams a tar bundle (`deploy/`, `config/`, `release.env`) over SSH to the host's forced command. The host verifies the signatures again, runs the service's deploy steps, watches an acceptance window and rolls back on its own.
5. **External check (optional).** It polls `ACCEPTANCE_URL`, if the environment sets one, and rolls back if it doesn't answer.

**What the caller's GitHub Environment holds:**

| Kind | Name | What |
|---|---|---|
| Secret | `SOPS_AGE_KEY` | The age key for that environment's SOPS files |
| Secret | `DEPLOY_SSH_KEY` | The SSH key the host accepts for that service |
| Variable | `DEPLOY_HOST` | Where to connect; the runner must reach it |
| Variable | `SSH_KNOWN_HOSTS` | The host's key line(s) |
| Variable | `DEPLOY_ACTORS` | Comma-separated GitHub logins allowed to deploy |
| Variable | `DEPLOY_USER` | Optional, default `deploy` |
| Variable | `ACCEPTANCE_URL` | Optional |

**Calling it.** Use `workflow-templates/service-deploy.yml`, with `secrets: inherit`, because environment secrets can't be passed to a reusable workflow explicitly. Pin the `uses:` ref to a full commit SHA.

**Free plan.** Environments and their secrets work on private repositories, but protection rules (required reviewers, branch policies) don't. The gate above, the host's own signature check and its per-service allowlist take their place.

The host side, the manifest format and the bundle are specified in `accentmd/infra` → `docs/deploy-contract.md` (private).
