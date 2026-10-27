# 01 — GitHub Actions basics

## Goal
## Key concepts (workflow, event, job, runner, step)
## What I built
## Commands I learned
## What I learned / surprised me

## Glossary
- **Workflow**: an automated process defined in a YAML file under `.github/workflows/` that runs one or more jobs.
- **Event**: something that happens (like a push, a pull request, or a new issue) that triggers a workflow.
- **Job**: a group of steps that run together on the same runner; jobs run in parallel unless you make them depend on each other.
- **Runner**: the machine (a GitHub-hosted or self-hosted server) that executes a job.
- **Step**: a single task inside a job, either a shell command or a reusable action.
- **Ruleset**: a set of repository rules (e.g. require pull requests or passing checks before merging) that GitHub enforces on branches or tags.
