## Week 10: Challenge Kickoff and Architecture Decision

**Sprint 5 Kickoff (Continuation) | Synchronous**

### Overview

Week 10 opens the DevSecOps challenge track. This is a planning and decision week, not a build week — no new CI steps ship yet. Your team will confirm what security gate already exists in your pipeline, evaluate the SBOM/vulnerability scanning, secrets-detection, and DAST tooling you'll add on top of it, run a baseline scan of the current codebase and image to see where you're starting from, and turn the Weeks 11-12 build into an estimated sprint backlog. By the end of the week you should have a signed architecture decision record, a baseline findings report, a test plan for the gates you're about to build, and a backlog ready for asynchronous work to start immediately in Week 11.

### Learning Objectives

- Evaluate and select SBOM/vulnerability scanning, secrets-detection, and DAST tooling as a team, with documented rationale
- Distinguish the merge gate your baseline CI already enforces from the new gates this track adds, so the team isn't re-solving a problem that's already solved
- Run a baseline security scan of the current codebase and container image and document findings without immediate remediation
- Convert a challenge track's goal into an estimated, ticketed sprint backlog

### Prerequisites

- Completed Weeks 1-9 work, available as a peer directory (this track's `app-stack` and `opentofu-setup` Ansible roles — copied into `week-11/ansible/roles/` — read your Week 2 Docker Compose stack and Week 4 OpenTofu config; see `week-11/ansible/site.yml`)
- Read access to your baseline `.github/workflows/ci.yml` — you are extending it, not replacing it
- Access to your team's shared, privileged container
- No new tooling installed yet — that starts in Week 11

### Sprint 5 Kickoff (Continuation)

This session is synchronous — get the whole team in the same room (or call) at once. Sprint 5 opened in Week 9; this session closes it out and opens Sprint 6, which runs through Weeks 11-12. The goal by the end of the session is a signed architecture decision, a baseline findings report, and a Sprint 6 backlog your DevOps, Security, and Software Engineer roles can start executing asynchronously next week.

---

### Part 1: Confirm What's Already Enforced

Before deciding what to build, make sure the whole team can answer: what does our pipeline already block, and since when?

**Step 1.** As a team, open `.github/workflows/ci.yml` and walk through it together.

**Step 2.** Confirm the team can answer these two questions before moving on:
- Since which week has a Trivy CRITICAL image scan run on every non-PR push? (Week 6.)
- What does the existing gate *not* cover? (Secrets committed to the repo, SAST findings in application code, and runtime/DAST behavior — none of that is scanned today.)

> **Enterprise Pattern:** Real security programs build gates in layers over time rather than one "big bang" pipeline. Knowing precisely which layer already exists (and since when) is what lets a team scope new work honestly instead of either duplicating a control that's already there or assuming coverage that doesn't exist.

---

### Part 2: Tool Decisions

**Decide as a team** — document your answers and rationale in `week-10/adr.md` (the Architecture Decision Record template already scaffolded in this directory):

- **SBOM and vulnerability scanning tool.** Example approaches: **Grype** (lightweight, fast, simple `--fail-on critical` flag) or **Trivy** (richer output, already used by the baseline `ci.yml` gate — reusing it may simplify tooling but won't automatically give you SBOM output without extra flags). Either tool needs to scan the real Flask image, `week-2-flask:latest` locally / `ghcr.io/<owner/repo, lowercased>/flask-app` once published — there is no local registry in this course, so don't design around one.
- **Secrets-detection tool.** Example approaches: **gitleaks** (scans git history, simple CLI, has an official GitHub Action) or **trufflehog** (deeper detection, verifies live credentials). Whichever you choose, decide now whether it scans full git history or just the current commit — that changes both runtime and what it can catch.
- **DAST tool.** Example approaches: **OWASP ZAP** baseline scan (the tool referenced throughout this track's materials) or an alternative if your architecture calls for it. Decide the scan target now: it will be the Week 2 Docker Compose stack at `http://localhost:8080`, which means whatever runs the scan needs that stack up and healthy first (see Week 12).
- **CI integration plan.** Decide, in writing: which of the three new checks block a merge immediately, and which stay advisory while the team builds confidence in them? The scaffolded `.github/workflows/security-scanning.yml` workflow already models one answer — all three advisory through Week 12, secrets detection blocking from Week 13, vulnerability detection blocking from Week 14 — but your team owns whether to keep that schedule or change it.

---

### Part 3: Baseline Reconnaissance

You can't show improvement without a starting point.

**Step 1.** Run your chosen secrets scanner against the current repository and your chosen SBOM/vulnerability scanner against the real `week-2-flask:latest` image, e.g.:

```bash
docker compose -f week-2/docker-compose.yml build
gitleaks detect --source . --verbose
grype week-2-flask:latest -o table
```

**Step 2.** Document whatever each scan finds — even "no findings" is a result worth recording — in `week-10/baseline-findings.md`. This establishes what Week 11-12 remediation, if any, is actually working against.

---

### Part 4: Sprint Planning

**Step 1.** Break the Weeks 11-12 deliverables into tickets: Week 11 (SBOM generation in CI, secrets detection blocking in CI, two test branches) and Week 12 (DAST scanning, CRITICAL vulnerability blocking, three test cases with evidence).

**Step 2.** Estimate each ticket and load them into `week-10/backlog.md` with at least 8 items, distributed so no single person can complete the sprint solo.

**Step 3.** Write the test plan for how you'll prove each gate actually blocks in `week-10/test-plan.md` — one test case per gate, with clear pass/fail criteria (this is what Week 11-12 execute against).

---

### Validation Checks

**QA runs all validation checks.** Nothing should be running yet this week beyond your baseline scans — Week 10 checks are about decisions and documentation, not new pipeline behavior.

#### Validation Check: Architecture Decision Signed Off

QA confirms `week-10/adr.md` has all sections filled in (not left as TODO placeholders) with a stated tool choice and rationale for SBOM/vulnerability scanning, secrets detection, and DAST, and that the whole team has reviewed it.

#### Validation Check: Baseline and Backlog Complete

QA confirms `week-10/baseline-findings.md` documents real scan output (not a blank template), `week-10/backlog.md` has at least 8 estimated items covering both Week 11 and Week 12 work, and `week-10/test-plan.md` defines a concrete pass/fail test case for each of the three gates.

---

### Deliverables

- [ ] `week-10/adr.md` complete and team-reviewed (tool choices for SBOM/vuln scanning, secrets detection, DAST, plus CI integration plan)
- [ ] `week-10/baseline-findings.md` documents real secrets and vulnerability scan output against the current repo and `week-2-flask:latest` image
- [ ] `week-10/backlog.md` has 8+ estimated tickets covering Weeks 11-12, distributed across roles
- [ ] `week-10/test-plan.md` defines a pass/fail test case for each of the three planned gates
- [ ] Team can state, without looking it up, what the baseline `ci.yml` Trivy gate already blocks and since when
- [ ] Sprint 5 retrospective (`docs/sprint-10-retrospective.md`) closed out
- [ ] QA report for the week (`docs/qa-report-10.md`) filled in with validation results

---

### Sprint Backlog: Preparing for Week 11

The Scrum Master should open the following tickets for Sprint 6:

- Add an SBOM generation step to `.github/workflows/security-scanning.yml` using the Week 10 tool decision, scanning `week-2-flask:latest`
- Add a secrets-detection step that runs before the image build and blocks on detection
- Create test branch `test/clean-merge` with a clean commit and confirm the pipeline passes
- Create test branch `test/secret-commit` with a fake secret and confirm the pipeline blocks it
- Record tool versions and both test run results directly in the Week 11 pipeline evidence (workflow run URLs, artifact links)

---
