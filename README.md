# Workflow Naming Guide

This repository contains reusable GitHub workflows under `.github/workflows/*.yaml`.

## Goals

- Keep names descriptive and consistent.
- Group jobs into MECE-style categories (mutually exclusive, collectively exhaustive).
- Avoid generic job IDs like `main`.

## Workflow file naming

Use lowercase kebab-case.

Pattern:

- `ci-<target>.yaml` for continuous integration workflows
- `release-<target>.yaml` for release/publish workflows
- `<purpose>.yaml` for cross-cutting utility workflows (for example `context.yaml`, `dependency-review.yaml`, `scorecard.yaml`, `retention-policy.yaml`)

Examples:

- `ci-typescript.yaml`
- `release-github-pages.yaml`
- `dependency-review.yaml`

## Job ID naming

Use lowercase kebab-case job IDs.

### Single-job workflows

For single-job workflows, the job ID should represent the MECE category for that workflow type:

- CI workflows: `continuous-integration`
- Security workflows: `security-analysis`
- Release workflows: `release`
- Maintenance workflows: `maintenance`
- Context workflows: `context`

This enables category grouping across workflows while the filename provides target-specific detail.

Examples:

- `ci-typescript.yaml` -> `continuous-integration`
- `dependency-review.yaml` -> `security-analysis`
- `release-github.yaml` -> `release`

### Multi-job workflows

Use distinct, phase-oriented names per job. Each job ID should describe one stage in the flow.

Good patterns:

- `context` / `prepare` / `build` / `publish` / `finalize`
- `analyze` / `plan` / `apply`

Avoid:

- `main`
- `job1`, `job2`

## Dependency and output wiring rules

When renaming a job ID, always update all references:

- `needs.<job-id>` references
- `jobs.<job-id>.outputs.*` mappings
- comments/doc snippets that encode job IDs (to prevent drift)

Checklist:

1. Rename `jobs.<old-id>` -> `jobs.<new-id>`.
2. Search for `<old-id>` in the same file and update all references.
3. Validate workflow syntax and run references before merging.

## Current category map

- `ci-*` -> `continuous-integration`
- `dependency-review.yaml`, `scorecard.yaml` -> `security-analysis`
- `release-github*.yaml`, `release-typescript.yaml` -> `release`
- `retention-policy.yaml` -> `maintenance`
- `context.yaml` -> `context`

## Notes

- Reusable workflows do not require a top-level `name:`.
- Caller workflows are responsible for user-facing workflow names in repository Actions views.
