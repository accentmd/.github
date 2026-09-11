# AGENTS.md — .github

Guidance for AI coding agents and contributors working in this repository.

## What this repo is

The organization's public `.github` repository. It holds the default issue templates for every `accentmd` repository, and later the org profile and generic reusable GitHub Actions workflows.

## Hard rules

1. **This repository is public.** Never add hostnames, IP addresses, secrets, internal URLs or environment-specific details.
2. **English only,** in files and commit messages.
3. **Labels referenced by templates must exist** in the repositories that use them. When a template label changes, rename the label in those repositories too (later managed by OpenTofu in `accentmd/infra`).
4. **Reusable workflows must stay generic.** Environment specifics are passed in as inputs or secrets by the calling repository.
5. **Self-hosted runners must never run jobs for this repository,** because anyone can open a pull request on a public repository.

## Conventions

- Conventional Commits (`chore: …`, `docs: …`), imperative and lower-case.
