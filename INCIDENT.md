# Incident: Required Opening Hours Heading Changed

## Summary

A controlled coursework test on `test-missing-heading` changed the heading in `hours.md` from `# Opening Hours` to `# Clinic Hours`. The automated check detected the change. Restoring the original heading produced a successful run.

This was a deliberate test, not a live clinic outage. The sections below follow the six-step method in the Student Reading Manual, Chapter 11, page 238.

## 1. Reproduce

The failure condition was deliberately introduced in commit `0730252` by renaming the required heading to `# Clinic Hours` and pushing the change.

[Run #5 failed](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407119922). This records one controlled failing run; repeated reproduction was not separately tested.

## 2. Read

The failed run reported:

> Missing required heading: # Opening Hours

> Process completed with exit code 1.

The first message identifies the unmet requirement. The exit code confirms failure but does not, by itself, explain the cause.

## 3. Isolate

The relevant step was `Check required clinic headings`, and the affected file was `hours.md`.

The heading check uses:

`grep -Fxq '# Opening Hours' hours.md`

The changed heading no longer matched the required complete line. The file still existed, and the workflow itself had not changed.

## 4. Hypothesize

Changing `# Opening Hours` to `# Clinic Hours` caused the exact-heading check to fail; restoring the required heading should make the same check pass.

## 5. Test One Change

Only the heading was restored to `# Opening Hours`, in commit `59d9dcc`. The workflow was left unchanged.

[Recovery run #6 passed](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407233767). The result supports the hypothesis because restoring the heading resolved the failure without changing the check.

## 6. Document

This incident record preserves the deliberate change, error messages, hypothesis, corrective change, and links to both runs. The test and recovery commits were included in pull request #4.

To prevent recurrence, the coordinator should preserve required headings when editing the content beneath them. Reviewers should inspect the changes and confirm successful checks before merging. Intentional heading changes should include a review of the corresponding requirement and automated check.

## Failure Category

**Category 6: A real defect**, using the categories in the Student Reading Manual, Chapter 11, page 246.

The deliberately edited content violated the agreed heading requirement. A specific check correctly failed with a meaningful message. The defect was in the content relative to that requirement.

This was not a missing-file failure: `hours.md` remained present. The missing item was a required line inside it.

## Limitations

The test demonstrates detection of a heading mismatch and recovery after correction. It does not prove that the listed opening hours or services are accurate. Those details still require manual review.
