# Incident: Required Opening Hours Heading Changed

## Summary

A controlled test on `test-missing-heading` changed the heading in `hours.md` from `# Opening Hours` to `# Clinic Hours`. This simulated an editor accidentally changing a required heading. The automated check detected the change. Restoring the original heading produced a successful run.

This was a coursework test, not a live clinic outage.

## 1. Identify the Problem

The Clinic content checks workflow failed after the heading was renamed in commit `0730252`.

## 2. Collect Evidence

[Failed run #5](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407119922) reported:

> Missing required heading: # Opening Hours

> Process completed with exit code 1.

The run was triggered by a push to `test-missing-heading`.

## 3. Form a Hypothesis

The check requires the exact line `# Opening Hours`. Changing that line to `# Clinic Hours` caused the content validation to fail.

## 4. Test One Change

Only the heading was restored to `# Opening Hours`, in commit `59d9dcc`. The workflow was left unchanged so the next run tested the proposed fix against the same requirement.

## 5. Verify Recovery

[Recovery run #6](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407233767) passed on the same branch.

The failed run followed by the successful recovery supports the hypothesis that the heading change caused the failure. Both runs are retained as evidence.

## 6. Prevent Recurrence

The coordinator should preserve required headings when updating the content beneath them. Before merging a change, the reviewer should check the file differences and confirm that the content checks pass.

If a heading needs to change intentionally, the content requirement and its automated check should be reviewed together.

## Failure Analysis and Limitations

The observed failure was a content validation failure: the edited file no longer met the exact heading requirement.

The check detected a structural change. It does not establish that the listed opening hours or services are factually correct; those still require manual review.
