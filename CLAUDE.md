# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

**Read `AGENTS.md` first.** It holds this repo's hard rules. Where this file and `AGENTS.md` disagree, follow `AGENTS.md`.

## What matters most here

- **Public repository.** Nothing internal: no hostnames, IPs, secrets or environment details.
- The issue templates in `.github/ISSUE_TEMPLATE/` are **org-wide defaults.** Their `labels:` must match labels that exist in each repository, and their text refers to the Accent Roadmap project's **Waiting** status and **Waiting on** field.
- English only.

## Git

- Conventional Commits, imperative and lower-case.
- No `Co-Authored-By` / Claude trailer. Never push; the user pushes. Rewriting history here needs `git push --force-with-lease`, because this repo has already been pushed.

## Platform context

The platform design lives in `accentmd/infra` → `docs/architecture/platform.md`.
