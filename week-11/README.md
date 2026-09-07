## Week 11: Core Build — SBOM Generation and Secrets Detection

**Sprint 6, Part 1 | Asynchronous**

### Overview

Week 11 is the first core build sprint for the DevSecOps track. Building on the tool decisions from Week 10, your team integrates SBOM generation and secrets detection into a real GitHub Actions workflow — `.github/workflows/security-scanning.yml`, already scaffolded with `secrets-detection`, `sbom-generation`, `vulnerability-scan`, and `summary` jobs — and proves both new checks work against real test branches. This is additive to the baseline `ci.yml` Trivy gate that's been running since Week 6, not a replacement for it: by the end of this week your repository will have two independent workflows watching different things.

### Learning Objectives

- Wire SBOM generation into a CI workflow that scans the real Flask application image, not a placeholder
- Wire secrets detection into a CI workflow so it blocks a merge on a real positive and passes on real clean code
- Reason about container image identity when there is no registry to lean on
- Produce and interpret CI evidence (workflow run URLs, artifact contents) as documentation, not just a passing checkmark

### Prerequisites

- Week 10 architecture decision (`week-10/adr.md`) signed off, naming your SBOM/vulnerability tool and secrets-detection tool
- Completed Weeks 1-9 work available as a peer directory, with the Week 2 Docker Compose stack buildable (`docker compose -f week-2/docker-compose.yml build`)
- Push access to test branches on the team's GitHub repository (secrets detection can't be proven without actually pushing a fake secret)

### Sprint 6, Part 1

Week 11 is asynchronous — no mandatory class time. Teams self-organize around CI integration, with daily async standups and code review on workflow changes. The ticket list from Week 10's backlog is your starting point.

---

### Part 1: SBOM Generation

**Step 1.** Confirm the `sbom-generation` job in `.github/workflows/security-scanning.yml` builds the real image and scans it. There is no registry anywhere in this course — local or CI — so the job builds `week-2-flask:latest` directly with `docker/build-push-action` (`push: false`, `load: true`) and scans it straight out of the runner's Docker daemon:

```yaml
- name: Generate SBOM
  run: |
    grype week-2-flask:latest -o table > sbom.txt
    cat sbom.txt
```

**Step 2.** Confirm the job runs after the image is built and does **not** fail the workflow (advisory only, per the Week 10 decision) — check that its `grype`/`trivy` invocation is followed by `|| true` or an equivalent non-blocking pattern.

**Step 3.** Confirm the SBOM output is retained as a workflow artifact (`actions/upload-artifact`), not just printed to the log and discarded.

> **Enterprise Pattern:** Production SBOM pipelines almost always start advisory-only. You want weeks of real output before you trust a scanner enough to let it block a merge — flipping the gate to blocking is a Week 13 decision, not a Week 11 one.

---

### Part 2: Secrets Detection

**Step 1.** Confirm the `secrets-detection` job runs gitleaks (or your Week 10 choice) against the full checkout (`fetch-depth: 0`, so history is scanned, not just the diff) and runs *before* the image build — a leaked credential shouldn't have to wait for a full build to be caught.

**Step 2.** Unlike SBOM generation, secrets detection is meant to block per the Week 10 CI integration plan. Update the job so it actually fails the workflow on a detection instead of swallowing the result — the scaffold currently runs `gitleaks detect --source . --verbose || true`, which reports but never fails. Decide with your team whether to flip this to blocking now or hold it advisory through Week 12 per the workflow's documented escalation schedule, and record which you chose and why in `docs/qa-report-11.md`.

**Step 3.** Set up branch protection on `main` requiring this check to pass, if your team decided to make it blocking now.

---

### Part 3: Prove It With Test Branches

**Step 1.** Create `test/clean-merge`, make a trivial clean commit, push it, and confirm both new jobs run and pass.

```bash
git checkout -b test/clean-merge
git commit --allow-empty -m "Test: clean commit for security-scanning pipeline"
git push origin test/clean-merge
```

**Step 2.** Create `test/secret-commit`, add an obviously fake secret, push it, and confirm the secrets-detection job catches it.

```bash
git checkout -b test/secret-commit
echo "aws_secret_access_key=AKIA1234567890ABCDEF" >> test-secret.txt
git add test-secret.txt
git commit -m "Test: fake secret for gitleaks detection"
git push origin test/secret-commit
```

**Step 3.** Record both workflow run URLs, the pass/fail result, and a screenshot in `docs/qa-report-11.md`.

---

### Validation Checks

**QA runs all validation checks.** By end of week, both new checks must have real, reproducible evidence — not a description of what they're supposed to do.

#### Validation Check: SBOM Generation

QA confirms the `sbom-generation` job in a recent `security-scanning.yml` run completed successfully, produced a non-empty SBOM artifact, and did not block the workflow.

#### Validation Check: Secrets Detection Blocks and Passes Correctly

QA opens the `test/clean-merge` and `test/secret-commit` workflow runs and confirms: the clean branch's secrets-detection job passed, and the secret branch's job caught the fake credential (visible in the job log or gitleaks report artifact) with a result consistent with the team's Week 10 decision on blocking vs. advisory.

---

### Deliverables

- [ ] `sbom-generation` job scans the real `week-2-flask:latest` image and uploads a non-empty SBOM artifact
- [ ] `secrets-detection` job runs before the image build against full git history
- [ ] Team decision on whether secrets detection blocks now or stays advisory is documented in `docs/qa-report-11.md`
- [ ] `test/clean-merge` branch pushed and both jobs pass
- [ ] `test/secret-commit` branch pushed and secrets detection catches the fake secret
- [ ] Both workflow run URLs and results recorded in `docs/qa-report-11.md`
- [ ] `docs/sprint-11-retrospective.md` filled in

---

### Sprint Backlog: Preparing for Week 12

The Scrum Master should open the following tickets for the second half of Sprint 6:

- Add a liveness precondition check before any DAST step (verify the Week 2 stack is actually up before scanning it)
- Add the DAST job to `security-scanning.yml` using the Week 10 tool decision
- Make the `vulnerability-scan` job's CRITICAL check blocking
- Create `test/vuln-critical-w12` and `test/secret-block-w12` test branches
- Re-verify `test/clean-build-w12` passes all four checks end-to-end

---
