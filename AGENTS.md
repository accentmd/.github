# AGENTS.md — .github

Guidance for AI coding agents and contributors working in this repository.

## What this repo is

The organization's public `.github` repository. It holds the org profile, the default issue and pull request templates, generic reusable GitHub Actions workflows (deploy, rollback, secret scan), starter workflow templates and the shared Renovate preset. See `README.md`.

## Hard rules

1. **This repository is public.** Never add hostnames, IP addresses, secrets, internal URLs or environment-specific details.
2. **English only,** in files and commit messages.
3. **Labels referenced by templates must exist** in the repositories that use them. When a template label changes, rename the label in those repositories too (later managed by OpenTofu in `accentmd/infra`).
4. **Reusable workflows must stay generic.** Environment specifics are passed in as inputs or secrets by the calling repository.
5. **Self-hosted runners must never run jobs for this repository,** because anyone can open a pull request on a public repository. `self-ci.yml` runs on GitHub-hosted runners only.
6. **Every action is pinned to a full commit SHA,** and every downloaded tool to a version and a checksum.
7. **No `${{ … }}` inside `run:` scripts.** Pass values through `env:` (template injection). `zizmor` checks this.
8. **The deploy workflow stays thin.** Deploy logic belongs on the host (`accent-deploy` in `accentmd/infra`). Changes to the bundle or the SSH commands must match `infra/docs/deploy-contract.md`, in the same change set.

## Conventions

- Conventional Commits (`chore: …`, `docs: …`, `feat(deploy): …`), imperative and lower-case.
- Before committing: `actionlint`, `zizmor --persona=regular .github/workflows/*.yml workflow-templates/*.yml` and `yamllint --strict .` (the same checks as `self-ci.yml`).
