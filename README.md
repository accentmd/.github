# .github

Shared issue templates for every repository in the organization.

Files in `.github/ISSUE_TEMPLATE/` act as defaults in every `accentmd` repository, including private ones, until a repository adds its own file of the same type. This repository is public because GitHub does not apply default community files from a private `.github` repository.

| Template | When | Label |
|---|---|---|
| `task.yml` | One pass, one pull request | `type: development` |
| `data-gap.yml` | Data is unavailable or unreliable | `type: data` |
| `external-request.yml` | Something is needed from someone outside the team | `type: external request` |

Work is tracked in the [Accent Roadmap](https://github.com/orgs/accentmd/projects) project.

Because this repository is public, it must never contain hostnames, IP addresses, secrets or environment-specific details.
