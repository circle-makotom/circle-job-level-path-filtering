# Job-level path filtering

This repository _experimentally_ demonstrates that job-level path filtering is possible using setup workflow.

This demonstration consists of two configs:

* `.circleci/config.yml`: The config for the setup workflow. This is generally intended to be a black box, and not expected to be actively reviewed and modified.
* `.circleci/continue-config.yml`: The config for the main workflow, and where user jobs are defined. This is where end users add modifications to optimize CI/CD operations for each module.

The setup workflow runs a JavaScript code that parses `.circleci/continue-config.yml`. The script filters out the jobs if the directories/files listed in `filters.paths` of those jobs have no changes against the `main` branch or the previous commit. For the demonstration purposes, the `job-level-path-filtering/do` job shows both the original `continue-config.yml` and the filtered config to be spooled (which is effective for the actual main workflow).

## Drawbacks

* Fan-ins of workflows may not work as expected. This is generally because the behaviour of fan-ins with filtered jobs is not obvious. (E.g., when job `C` requires jobs `A` and `B`, and `A` is filtered out, should `C` run?)
* `continue-config.yml` is always an invalid config, because it contains `filters.paths` objects. Config version 2.1 does not accept custom properties for workflows (because it is impossible to distinguish job parameters and metadata-ish properties).
