# Riverside Clinic: Delivery Improvement Report

## Summary for the Clinic Manager

The project establishes a controlled process for changing the fictional clinic’s website content. Opening hours and services are stored in GitHub, changes are recorded through branches and pull requests, and automated checks run on pushes and pull requests. This project checks content; it does not deploy a live website.

The process addresses the manager’s three concerns:

- **Incorrect opening hours:** Checks detect missing or empty content files and missing required headings. A person must still confirm that the actual hours are correct.
- **Dependence on one contractor:** The plan, pipeline explanation, and troubleshooting record give another maintainer instructions and examples to follow.
- **Evidence for the grant board:** Commits, pull requests, and workflow results provide a record of changes and validation. Independent human approval is a proposed improvement, not an established control.

## Results and Expected Improvements

A controlled test renamed the opening-hours heading. [Run #5 failed](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407119922), identifying the missing required heading. Restoring the heading without changing the workflow produced [successful run #6](https://github.com/m1n1m0n0/riverside-clinic-devops/actions/runs/36407233767).

Two DORA metrics could improve if this process is adopted for real releases. **Change failure rate** could decrease because checks and review can catch some errors before release. **Time to restore service** could decrease because version history and documented troubleshooting make diagnosis and recovery easier. These are expected benefits, not measured improvements: the coursework test was not a production incident, and no live deployments were measured.

## AI Assistance and Verification

I used an AI assistant for step-by-step GitHub guidance, workflow code, and documentation wording. One complete text prompt I sent was:

> what to do

This followed screenshots showing the project’s progress; the assistant used the conversation context to suggest the next steps. The assistance was substantial, including the workflow and report structure.

I tested the suggested workflow by changing the required heading, observing the failure, restoring the heading, and confirming a successful run. This verified the opening-hours heading check. It did not test every failure condition or prove that clinic information was accurate. I also checked the incident report against Chapter 11 of the Student Reading Manual and aligned it with the six-step method and Category 6, “A real defect.”

## Reflection

1. **What I learned:** I learned to use GitHub branches, create pull requests, merge changes, and add automated tests with GitHub Actions.
2. **Main challenge:** Setting up GitHub Actions was the hardest part.
3. **How I addressed it:** With AI guidance, I added the workflow on a branch, checked its successful run, and merged it through a pull request. I then deliberately changed a required heading, examined the failure, restored the heading, and confirmed recovery.
4. **Next eight hours:** I would strengthen the schedule checks, test the other failure conditions, arrange independent review, and configure required checks before merging.
5. **Workplace application:** I can use this process at work to record changes on branches, run automated checks, and review pull requests before merging code.

Approximate time spent: **4 hours**.
