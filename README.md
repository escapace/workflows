# GitHub Automation Guide

This repository stores reusable GitHub workflows and composite actions under `.github/`. The guide should stay aligned with the current files to avoid drift in job IDs, outputs, and caller references.

## Layout

- `.github/workflows/*.yaml` contains reusable workflows.
- `.github/actions/*/action.yaml` contains composite actions used by those workflows.
- `.github/dependabot.yaml` manages GitHub Actions dependency updates.

## Workflow file names

Use lowercase kebab-case.

- `ci-<target>.yaml` for continuous integration workflows
- `release-<target>.yaml` for release and publish workflows
- `<purpose>.yaml` for cross-cutting workflows such as `auto-merge.yaml`, `codeql.yaml`, `context.yaml`, `dependency-review.yaml`, `retention-policy.yaml`, and `scorecard.yaml`

## Job IDs

Use lowercase kebab-case job IDs.

### Single-job workflows

Prefer stable category IDs when a workflow has one job.

| Workflow                    | Job ID                   |
| --------------------------- | ------------------------ |
| `ci-ansible-role.yaml`      | `continuous-integration` |
| `ci-terraform-module.yaml`  | `continuous-integration` |
| `ci-typescript.yaml`        | `continuous-integration` |
| `codeql.yaml`               | `codeql-analysis`        |
| `context.yaml`              | `context`                |
| `dependency-review.yaml`    | `security-analysis`      |
| `release-github-pages.yaml` | `release`                |
| `release-github.yaml`       | `release`                |
| `retention-policy.yaml`     | `maintenance`            |
| `scorecard.yaml`            | `security-analysis`      |

`codeql.yaml` intentionally uses `codeql-analysis` instead of `security-analysis`. The workflow has its own permission model and caller contract.

### Multi-job workflows

Use phase-oriented job IDs that describe one step in the flow.

| Workflow                  | Job IDs                                  |
| ------------------------- | ---------------------------------------- |
| `auto-merge.yaml`         | `discover`, `auto-merge`                 |
| `release-container.yaml`  | `prepare`, `build`, `publish`            |
| `release-typescript.yaml` | `publish-github-packages`, `publish-npm` |

Avoid generic IDs such as `main`, `job1`, or `job2`.

## Local action names

Use lowercase kebab-case for directories under `.github/actions/`.

Composite actions:

- `bake-containers`
- `discover-containers`

If an action name, input, or output changes, update every workflow that calls it.

## Rename checklist

1. Rename the workflow file, job ID, or action directory.
2. Update `needs.<job-id>` references.
3. Update `jobs.<job-id>.outputs.*` references.
4. Update `uses:` references for local actions and reusable workflows.
5. Validate workflow syntax and caller references before merging.

## Notes

- Reusable workflows do not need a top-level `name:`.
- Caller workflows own the display name shown in the GitHub Actions UI.
