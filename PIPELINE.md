# Clinic Content Pipeline

## Purpose

The pipeline checks the structure of the fictional clinic content whenever changes are pushed or a pull request is opened or updated. It helps identify missing content files and changed required headings before a reviewer approves the change.

It does not publish a website or verify that the opening hours and services are factually correct. Those details still need manual review.

## Workflow File

Location: `.github/workflows/content-checks.yml`

```yaml
name: Clinic content checks

on:
  push:
  pull_request:

permissions:
  contents: read

jobs:
  check-content:
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v6

      - name: Check required content files
        shell: bash
        run: |
          set -euo pipefail
          for file in hours.md services.md; do
            if [ ! -s "$file" ]; then
              echo "::error file=$file::Required content file is missing or empty."
              exit 1
            fi
            echo "PASS: $file exists and is not empty."
          done

      - name: Check required clinic headings
        shell: bash
        run: |
          set -euo pipefail

          if ! grep -Fxq '# Opening Hours' hours.md; then
            echo "::error file=hours.md::Missing required heading: # Opening Hours"
            exit 1
          fi

          if ! grep -Fxq '## Available Services' services.md; then
            echo "::error file=services.md::Missing required heading: ## Available Services"
            exit 1
          fi

          echo "PASS: Both required clinic headings are present."
```

## How It Works

| Setting or command | Explanation |
|---|---|
| `name` | Gives the workflow its label in the Actions tab. |
| `on: push` | Runs the checks when commits are pushed, including changes on working branches. |
| `on: pull_request` | Runs the checks when a pull request is opened or updated. |
| `permissions: contents: read` | Gives the workflow token read access to repository content. These checks do not need write access. |
| `jobs: check-content` | Defines one job containing the three steps. |
| `runs-on: ubuntu-latest` | Runs the job on a GitHub-hosted Ubuntu environment. |
| `steps` | Lists the operations in execution order. |
| `actions/checkout@v6` | Makes the repository files available to the job. |
| `shell: bash` | Runs the following commands using Bash. |
| `run: \|` | Starts a block containing multiple command lines. |
| `set -euo pipefail` | Makes unexpected command errors, unset variables, and failures within command pipelines easier to detect. |
| `for file in hours.md services.md` | Checks each of the two required content files. |
| `[ ! -s "$file" ]` | Detects a missing file or a file with zero bytes. It does not detect a file containing only whitespace. |
| `if`, `then`, and `fi` | Enclose the commands to run when a failure condition is detected. |
| `echo "::error ..."` | Adds a readable error annotation identifying the affected file. |
| `exit 1` | Stops the step with a failure status. |
| `done` | Ends the loop over the two files. |
| `grep -Fxq` | Searches quietly for a literal, complete line matching the required heading. |
| `! grep ...` | Enters the error branch when the required heading is not found. |
| `echo "PASS: ..."` | Records a success message after the relevant checks pass. |

## Clinic-Specific Check

The custom check requires the exact line `# Opening Hours` in `hours.md` and `## Available Services` in `services.md`.

This protects the agreed structure of the clinic's opening-hours and services information. An accidental heading change becomes visible through a failed check and an error naming the affected file.

A successful check does not prove that the schedule is complete or accurate. A reviewer must still read the content.

## Evidence That the Check Works

The heading in `hours.md` was deliberately changed from `# Opening Hours` to `# Clinic Hours` on `test-missing-heading`.

- [Failed run #5](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407119922): commit `0730252` failed with `Missing required heading: # Opening Hours`.
- [Successful recovery run #6](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407233767): commit `59d9dcc` passed after the required heading was restored.

The workflow was unchanged during this test. This demonstrates the opening-hours heading check's failure and recovery behavior. The missing-file, empty-file, and services-heading failure cases have not been deliberately tested.

The troubleshooting sequence is recorded in [INCIDENT.md](INCIDENT.md). Pull request #4 also showed successful runs for both the push and pull-request triggers.

## Two Next Improvements

### 1. Require Checks and Review Before Merging

Lifecycle stage: Release.

Configure a repository rule requiring the content check to pass and an independent reviewer to approve changes before they enter `main`, once a second reviewer is available.

The current workflow reports failures, but the workflow file alone does not prevent someone from merging. A required check and review would make the release decision more consistent and provide clearer evidence of approval.

### 2. Check the Weekly Schedule Structure

Lifecycle stage: Test.

Extend validation to detect missing weekdays or blank opening-hours entries. Add controlled tests showing that missing information fails and complete information passes.

The existing heading check can pass even when schedule details are incomplete. This improvement would catch more accidental omissions, while manual review would remain responsible for confirming the actual hours.
