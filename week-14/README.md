## Week 14: Demo Day — Final Rebuild and Presentation

**Sprint 7, Part 2 | Synchronous — Capstone**

### Overview

Week 14 is Demo Day, the capstone of the DevSecOps track. In front of an audience, your team wipes the container and Kubernetes cluster, rebuilds the entire environment from a clean state using the Ansible playbook you finished in Week 13, verifies the security tooling live, and walks through the CI/CD pipeline showing all three gates — SBOM (advisory), secrets detection, and CRITICAL vulnerability blocking — behaving correctly. This proves, on stage, that the environment is genuinely defined as code: no manual setup, no "it works on my machine."

### Learning Objectives

- Execute a live, unscripted-but-rehearsed infrastructure rebuild in front of an audience
- Present and defend security tooling and gate decisions to non-team stakeholders
- Demonstrate the practical value of idempotent infrastructure-as-code under real time pressure
- Reflect on the track as a whole and identify what would change with more time

### Prerequisites

- Week 13 dry run and demo rehearsal completed, with real timings recorded
- `security-scanning` role wired into `week-11/ansible/site.yml` and proven idempotent
- All three test branches (`test/clean-build-w12`, `test/secret-block-w12`, `test/vuln-critical-w12`) confirmed to still show correct pass/fail status
- At least 10GB free disk space on the demo machine, and a fallback plan for the most fragile step identified in Week 13

### Sprint 7, Part 2 — Demo Day

This is a live, synchronous, timed presentation — typically 10-15 minutes. Everyone on the team presents their assigned section from the Week 13 rehearsal.

---

### Part 1: Environment Wipe

**Step 1.** Show the current state to the audience before wiping anything:

```bash
kubectl get pods -n default
docker ps
```

**Step 2.** Wipe container storage and reset the cluster:

```bash
docker system prune -a --volumes
k3d cluster delete myapp
k3d cluster create myapp --agents 2 --port "8081:80@loadbalancer" --k3s-arg "--disable=traefik@server:0"
```

> **Troubleshooting:** If `k3d cluster create` hangs and
> `docker logs k3d-myapp-server-0` shows repeated `"too many open files"` /
> `"error creating fsnotify watcher"` errors, the host has run out of inotify watch
> instances — a common RHEL default (`fs.inotify.max_user_instances=128`) is too low
> for a fresh k3s cluster. Fix: `sudo sysctl -w fs.inotify.max_user_instances=1024`,
> then delete and retry cluster creation.

**Narration:** explain to the audience that this is equivalent to a brand-new machine — no state or configuration survives.

---

### Part 2: Playbook Rebuild

**Step 1.** Run the full playbook live:

```bash
cd week-11/ansible
ansible-playbook -i inventory site.yml
```

**Step 2.** Narrate what's happening as each play runs: baseline package setup, `app-stack` (Docker Compose), `k3d-setup` (the `myapp` cluster), `opentofu-setup` (OpenTofu install and init), and finally `security-scanning` (this track's addition — installing your chosen SBOM/vulnerability and secrets-detection tools).

> **Enterprise Pattern:** The reason this playbook is a superset built up week by week, rather than one script written at the end, is that each week's addition was independently tested against a real environment. That's what makes a full rebuild on stage a reasonable thing to attempt live at all.

---

### Part 3: Tool Verification

Run the verification commands live, on screen:

```bash
which grype 2>/dev/null && grype --version || which trivy 2>/dev/null && trivy --version
which gitleaks 2>/dev/null && gitleaks version || which trufflehog 2>/dev/null && trufflehog version
```

---

### Part 4: CI/CD Pipeline Walkthrough

**Step 1.** Open the repository's GitHub Actions tab and navigate to `security-scanning.yml`'s run history.

**Step 2.** Show all three test branches and their outcomes:
- `test/clean-build-w12` — green, all four checks (SBOM, secrets, vulnerability scan, DAST) pass
- `test/secret-block-w12` — red, blocked at secrets detection
- `test/vuln-critical-w12` — red, blocked at the vulnerability scan, with the CVE ID visible

**Step 3.** Also point out the baseline `ci.yml` Trivy CRITICAL gate, running since Week 6 — make clear to the audience that this track added three new gates on top of an existing one, rather than building security tooling from nothing.

---

### Part 5: Q&A and Wrap-Up

Be ready to answer:
- Why this SBOM/vulnerability tool and secrets detector, over the alternatives considered in Week 10?
- What happens when a developer's merge gets blocked — what's the remediation path?
- How much slower is the pipeline with these gates than without them?
- What would the team do differently with another sprint?

---

### Validation Checks

**QA runs all validation checks** during the Week 13 rehearsal — Demo Day itself is the live execution of those already-validated checks, not a first attempt.

#### Validation Check: Full Rebuild Succeeds Live

The playbook completes with exit code 0, all roles report success, and both security tools verify correctly, all performed live rather than pre-recorded.

#### Validation Check: All Three Gates Demonstrated

Each of the three test branches is opened live and its actual pass/fail status matches what's being claimed to the audience — this was already confirmed in Week 13, and Demo Day simply re-shows it.

---

### Deliverables

- [ ] Live environment wipe and rebuild completed with no manual intervention
- [ ] Both security tools verified live, output visible to the audience
- [ ] All three test branches shown with correct, matching pass/fail status
- [ ] Baseline `ci.yml` gate and this track's three new gates both explained to the audience
- [ ] Demo completed within the allocated time slot
- [ ] `docs/qa-report-14.md` and `docs/sprint-14-retrospective.md` filled in and signed

---

### Wrap-Up: Track Retrospective

This is the last week of the DevSecOps track — there is no Week 15. Close out as a team:

- **What shipped:** a `security-scanning` CI workflow with SBOM generation, secrets detection, DAST scanning, and CRITICAL vulnerability blocking, all layered on top of the baseline `ci.yml` Trivy gate that's been running since Week 6; an idempotent `security-scanning` Ansible role that installs the same tooling locally; and a proven, wiped-and-rebuilt environment.
- **What to reflect on as a team:** which gate decisions (advisory vs. blocking, which tool) would you revisit with more time? What part of the Week 13 dry run was the most fragile, and why? Would the SBOM/DAST steps scale to a larger application, or were shortcuts taken that wouldn't hold up?
- **What carries forward:** the security gates remain active in `security-scanning.yml` and `ci.yml` going forward — this isn't a one-time demo artifact, it's the pipeline the codebase now runs under permanently.

Sign off Sprint 7 in `docs/sprint-14-retrospective.md`, and confirm `docs/qa-report-14.md` reflects the actual, live Demo Day outcome — not the Week 13 rehearsal's.

---
