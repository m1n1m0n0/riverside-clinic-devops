# Riverside Clinic Delivery Plan

## Current Process

The office coordinator prepares website content, and a part-time contractor edits files directly on the live server on Thursday evenings. There is no version history or documented procedure. Recovery depends on choosing an occasional backup folder whose name may not identify the latest working version. About one change in four needs to be reversed the next morning. This risks showing incorrect opening hours, makes the clinic dependent on one contractor, and leaves little evidence of controlled changes for the grant board.

## Current DevOps Lifecycle

This table assesses the original scenario, before this project's improvements.
“Absent” means no practice is described in the scenario.

| Stage | Current status | Evidence |
|---|---|---|
| Plan | Manual | Content changes are prepared by staff and scheduled for Thursday evenings; no documented backlog is described. |
| Code | Manual | The contractor edits website files directly on the live server. |
| Build | Absent | No repeatable process packages or prepares a release. |
| Test | Absent | No checks before publication are described; errors are discovered after changes go live. |
| Release | Manual | The contractor decides which live edits to publish, without a recorded approval or version. |
| Deploy | Manual | Live files are edited directly; recovery involves uploading a backup folder. |
| Operate | Manual | The contractor handles recovery using whichever backup seems most recent. |
| Monitor | Absent | No systematic monitoring is described, although staff discover problems the following morning. |

## CALMS Assessment

### Culture
The coordinator and contractor have separate responsibilities, but operational knowledge is concentrated in the contractor. A shared review process would make responsibility clearer.

### Automation
Changes and recovery are manual, with no automated verification described. Content checks could catch missing sections before publication, although they cannot prove that opening hours are factually correct.

### Lean
About one change in four requires reversal, creating avoidable rework. Smaller reviewed changes would make problems easier to identify and correct.

### Measurement
The scenario gives an approximate reversal rate, but no regular collection of delivery or recovery measurements is described. Recording change outcomes would allow the clinic to assess whether the process improves.

### Sharing
Nothing is documented, and the contractor is the only person who understands the process. Sharing is the weakest element because another person cannot reliably maintain or recover the site, and the board lacks an auditable procedure.

## User Stories

### 1. Clear Opening Hours
As a patient, I want clear opening hours so that I can plan my visit.

Acceptance criteria:
- hours.md contains an Opening Hours heading and a schedule covering every day of the week.
- Holiday information is present.
- The pull request records a manual review of the hours before merging.

### 2. Reviewable Changes
As the clinic manager, I want a record of proposed changes so that I can show the board how updates are controlled.

Acceptance criteria:
- Each content change is proposed through a pull request that explains its purpose.
- The pull request shows the changed files and the automated check result.
- Merged changes remain visible in repository history.

### 3. Automatic Content Checks
As the office coordinator, I want missing content to be flagged automatically so that incomplete updates can be corrected before publication.

Acceptance criteria:
- The workflow runs on pushes and pull requests.
- It checks that hours.md and services.md exist and are not empty.
- It checks for the Opening Hours heading and the Available Services section.
- Removing a required heading produces a failed run, and restoring it produces a passing run.

### 4. Repeatable Recovery
As a replacement maintainer, I want a documented troubleshooting and recovery process so that I can resolve a failed change without depending on the original contractor.

Acceptance criteria:
- INCIDENT.md identifies the faulty change and quotes the actual failing log.
- It records one specific hypothesis, one corrective change, and the resulting run.
- It explains how to locate a previous working change and propose a reversal through a pull request.
- It includes a prevention action that the coordinator can follow.

## Definition of Done

- The change meets its user story's acceptance criteria.
- Content is fictional and contains no personal data or credentials.
- The change has meaningful commits on a separate branch.
- The pull request explains the change and records manual review.
- The configured automated checks pass before merging.
- Relevant documentation is updated and the change history is retained.

## Scope

This project demonstrates controlled changes and automated verification using placeholder content. It does not deploy a live clinic website or collect appointment requests. Automated checks verify file structure; a person must still review the accuracy of opening hours and service information.
