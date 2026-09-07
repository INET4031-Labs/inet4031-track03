## Week 12: Integration Testing and DAST

**Sprint 6, Part 2 | Asynchronous**

### Overview

Week 12 closes out Sprint 6. Your team adds Dynamic Application Security Testing (DAST) to `security-scanning.yml`, flips the CRITICAL vulnerability check from advisory to blocking, and produces the evidence that all three gates — SBOM (advisory), secrets detection, and CRITICAL vulnerability scanning — behave correctly under three concrete test cases: a clean pass and two deliberate blocks. By the end of the week the pipeline should reject exactly the things it's supposed to reject, and nothing else.

### Learning Objectives

- Integrate a DAST scan into CI against a real running target, including a liveness precondition so the scan fails loudly instead of silently no-opping
- Convert a vulnerability scanner from advisory to a real merge-blocking gate
- Design and execute negative test cases (deliberately broken input) alongside positive ones
- Produce auditable evidence — workflow URLs, screenshots, CVE IDs — for each gate, not just a claim that it works

### Prerequisites

- Week 11 complete: SBOM generation and secrets detection both running in `security-scanning.yml`, with `test/clean-merge` and `test/secret-commit` evidence recorded
- The Week 2 Docker Compose stack must be buildable and startable locally (`docker compose -f week-2/docker-compose.yml up -d`) — the DAST target is `http://localhost:8080`, the real nginx-fronted Flask stack, not a placeholder URL
- Your Week 10 DAST tool decision (OWASP ZAP or alternative)

### Sprint 6, Part 2

Week 12 continues the asynchronous pattern from Week 11: daily async standups, code review on workflow changes, and thorough test execution. This week's work depends on Week 11's jobs already being in place — you're adding to `security-scanning.yml`, not starting a new workflow.

---

### Part 1: DAST — With a Real Liveness Check

A DAST scan pointed at a dead target doesn't fail — it just produces an empty "clean" report, which is worse than no report at all because it looks like a pass.

**Step 1.** Before adding the ZAP step itself, add a precondition step that verifies the target stack is actually up and fails the job explicitly if it isn't:

```yaml
- name: Verify target stack is healthy before scanning
  run: |
    docker compose -f week-2/docker-compose.yml ps
    if ! curl -sf http://localhost:8080/ -o /dev/null; then
      echo "::error::Target http://localhost:8080 is not responding — Week 2 stack is not up. Aborting DAST scan."
      exit 1
    fi
    echo "Target is live, proceeding with DAST scan."
```

**Step 2.** Add the ZAP baseline scan (or your Week 10 alternative) against `http://localhost:8080`, scoped to OWASP Top 10 checks:

```yaml
- name: Run OWASP ZAP Scan
  run: |
    docker run --rm -v $(pwd):/zap/wrk:rw -t owasp/zap2docker-stable \
      zap-baseline.py -t http://localhost:8080 -r dast-report.html
```

**Step 3.** Confirm the DAST step stays advisory (does not block the merge) per the Week 10 decision, and that `dast-report.html` is uploaded as a workflow artifact.

> **Enterprise Pattern:** "Fail loudly, not silently" is a core reliability principle that applies just as much to security tooling as to application code — a security check nobody notices is broken provides negative value, since it creates false confidence.

---

### Part 2: Flip Vulnerability Scanning to Blocking

**Step 1.** In the `vulnerability-scan` job, change the CRITICAL check from advisory (`|| true`) to blocking — the scan should exit non-zero, and the job should be allowed to actually fail:

```yaml
- name: Scan Image for CRITICAL Vulnerabilities
  run: |
    grype week-2-flask:latest --fail-on=critical
```

**Step 2.** Confirm branch protection on `main` requires this job to pass (add it if it isn't there yet).

**Step 3.** Confirm secrets detection from Week 11 is still blocking (or flip it now if your team deferred that decision) — Week 12 is your last chance to catch a regression before Week 13's Ansible integration and Week 14's demo.

---

### Part 3: Three Test Cases

Push all three and record the workflow run URL, result, and (for the two blocking cases) the exact failure message for each.

**Test Case 1 — Clean build, all gates pass (`test/clean-build-w12`):**
```bash
git checkout -b test/clean-build-w12
git commit --allow-empty -m "Test: clean build, all gates should pass"
git push origin test/clean-build-w12
```
Expect: SBOM, secrets detection, vulnerability scan, and DAST all pass; merge allowed.

**Test Case 2 — CRITICAL vulnerability blocks (`test/vuln-critical-w12`):**
Temporarily point the Dockerfile at a base image with a known CRITICAL CVE (e.g., an old, unpatched `python` or `ubuntu` tag), push, and confirm the vulnerability-scan job fails with the CVE ID visible in the log.
```bash
git checkout -b test/vuln-critical-w12
# edit week-2/app/Dockerfile to use a known-vulnerable base image tag
git commit -am "Test: base image with known CRITICAL CVE"
git push origin test/vuln-critical-w12
```
Expect: vulnerability scan fails, merge blocked, CVE ID logged.

**Test Case 3 — Secret commit blocks (`test/secret-block-w12`):**
```bash
git checkout -b test/secret-block-w12
echo "password=super_secret_credentials_12345" >> config.txt
git add config.txt && git commit -m "Test: fake secret"
git push origin test/secret-block-w12
```
Expect: secrets detection fails, merge blocked.

---

### Validation Checks

**QA runs all validation checks.** All three test cases must have real workflow evidence, not a description of expected behavior.

#### Validation Check: DAST Runs Against a Live Target

QA confirms the precondition step actually ran (visible in the job log) before the ZAP step, that the Week 2 stack was genuinely up during the scan, and that `dast-report.html` was uploaded with real findings (not empty).

#### Validation Check: All Three Test Cases

QA opens all three test branch workflow runs and confirms: `test/clean-build-w12` is fully green, `test/vuln-critical-w12` fails specifically at the vulnerability-scan job with a CVE ID logged, and `test/secret-block-w12` fails specifically at the secrets-detection job. Any case that fails at the wrong step, or fails to fail, is rework.

---

### Deliverables

- [ ] DAST liveness precondition step added and confirmed to fail loudly when the target is down
- [ ] `vulnerability-scan` job flipped from advisory to blocking on CRITICAL severity
- [ ] `test/clean-build-w12`: all four checks pass, merge allowed
- [ ] `test/vuln-critical-w12`: vulnerability scan blocks, CVE ID documented
- [ ] `test/secret-block-w12`: secrets detection blocks
- [ ] All three workflow run URLs and results recorded in `docs/qa-report-12.md`
- [ ] `docs/sprint-12-retrospective.md` filled in
- [ ] `week-12/demo-plan.md` drafted (even roughly) as a starting point for Week 13's dry run

---

### Sprint Backlog: Preparing for Week 13

The Scrum Master should open the following tickets for Sprint 7:

- Author `week-11/ansible/roles/security-scanning/tasks/main.yml` to install the team's chosen SBOM/vulnerability and secrets-detection tools locally, idempotently
- Add `security-scanning` to `week-11/ansible/site.yml`'s roles list, after `app-stack`, `k3d-setup`, and `opentofu-setup`
- Run `ansible-playbook -i inventory site.yml --syntax-check` and fix any errors
- Schedule and script the Week 13 dry-run and demo rehearsal in `week-12/demo-plan.md`

---
