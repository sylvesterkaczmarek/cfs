# cFS GitHub Actions Workflows

This document describes the workflows and reusable actions currently maintained on the `dev` branch.

GitHub currently reports the repository workflows below as active. Reusable workflows are invoked by cFS subrepositories or other workflows and may not run directly on every push or pull request.

## Active workflows

| Workflow | File | Purpose |
| --- | --- | --- |
| Changelog | `changelog.yml` | Generate the cFS changelog. |
| Format Check | `format-check.yml` | Run source-format validation. |
| CodeQL Analysis: cFS-Bundle | `codeql-analysis.yml` | Run CodeQL for the cFS bundle. |
| CodeQL Reusable Workflow | `codeql-reusable.yml` | Reusable CodeQL analysis for cFS repositories. |
| Update wiki | `cfs-wiki.yml` | Update the cFS wiki. |
| MCDC Analysis | `mcdc.yml` | Run MC/DC analysis for the cFS bundle. |
| Build and execute CFS with multiple configurations | `test-cfs-qemu.yml` | Build and execute cFS test configurations. |
| Update CFS bundle | `update-bundle.yml` | Update bundle component references. |
| Build And Run Reusable Workflow | `build-run-app-reusable.yml` | Build and run an application with cFE. |
| Add Issues or PRs to Project Reusable Workflow | `add-to-project-reusable.yml` | Reusable project-board automation. |
| Add Issue or PR to Project | `add-to-project.yml` | Add issues and pull requests to the project board. |
| Build and Test using multitarget makefile | `build-cfs-multitarget.yml` | Exercise the multitarget build. |
| Build Document Reusable Workflow | `build-doc-reusable.yml` | Reusable documentation build. |
| MCDC Reusable Workflow | `mcdc-reusable.yml` | Reusable MC/DC analysis. |
| Unit Test and Coverage Reusable Workflow | `unit-test-coverage-reusable.yml` | Reusable unit-test and coverage analysis. |
| Static Analysis Reusable Workflow | `static-analysis-reusable.yml` | Reusable cppcheck static analysis. |
| Build COSMOS gem | `cosmos-gem.yml` | Build the cFS COSMOS integration gem. |
| Build COSMOS tests | `cosmos-test.yml` | Build and execute COSMOS integration tests. |

The workflow list and status can also be checked from the repository's **Actions** tab. Keeping this table aligned with the files on `dev` avoids documenting removed workflow names or obsolete trigger behavior.

## Reusable workflow architecture

Several workflows are deliberately centralized in cFS so that subrepositories can invoke the same implementation rather than carrying copies. These include CodeQL, static analysis, MC/DC, unit-test coverage, documentation, application build/run, and project-board automation.

![Reusable Workflows Architecture](./Reusable-Workflows-Architecture.svg)

## Repository composite actions

The workflows also use reusable composite actions maintained in this repository under `actions/`:

| Action | Path | Purpose |
| --- | --- | --- |
| Build app | `actions/build-app` | Build an application and package its artifacts. |
| cppcheck | `actions/cppcheck` | Run cppcheck and publish a static-analysis summary. |
| Health-check logs | `actions/healthcheck-logs` | Check runtime logs for expected startup output. |
| Setup app | `actions/setup-app` | Assemble cFE, dependencies, configuration, and application source for a test build. |
| Start cFS container | `actions/start-cfs-container` | Start a cFS execution container for runtime tests. |
| Stop cFS container | `actions/stop-cfs-container` | Stop the execution container created by the test workflow. |

Individual workflows additionally use GitHub and third-party actions such as `actions/checkout`, `actions/upload-artifact`, `actions/download-artifact`, the GitHub CodeQL actions, and duplicate-run filtering. The workflow files are the source of truth for the exact action version used by each job.

## Key analysis workflows

### CodeQL

`codeql-analysis.yml` invokes the reusable CodeQL workflow for the cFS bundle. CodeQL results are uploaded to GitHub code scanning and can be reviewed from the repository's **Security** tab.

### Static analysis

`static-analysis-reusable.yml` runs the repository `actions/cppcheck` composite action. The cppcheck action writes its result table to the GitHub job summary and uploads the raw analysis output as the `cppcheck-errors` artifact.

### Format check

`format-check.yml` validates source formatting against the repository formatting rules.

### Unit test, coverage, and MC/DC

`unit-test-coverage-reusable.yml` provides shared unit-test and coverage behavior, while `mcdc-reusable.yml` provides the reusable MC/DC analysis used by the bundle-level workflow.

## Build and integration workflows

`build-run-app-reusable.yml` provides a common build-and-run path for cFS applications. `build-cfs-multitarget.yml` exercises the multitarget makefile, and `test-cfs-qemu.yml` builds and executes multiple cFS configurations.

COSMOS integration is covered by `cosmos-gem.yml` and `cosmos-test.yml`. Bundle updates are handled by `update-bundle.yml`, and documentation builds can use `build-doc-reusable.yml`.

## Maintenance

When adding, renaming, disabling, or removing a workflow or repository composite action, update this document in the same pull request. This keeps the workflow inventory on `dev` synchronized with the actual automation.
