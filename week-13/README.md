## Week 13: Finalization and Ansible Dry Run

**Sprint 7, Part 1 | Synchronous**

### Overview

Week 13 is the synchronous finalization sprint. With both gates (SBOM/secrets in Week 11, DAST/vulnerability blocking in Week 12) proven in CI, your team now authors a real `security-scanning` Ansible role, adds it to `week-11/ansible/site.yml` in the superset pattern used since Week 2, and proves the whole playbook — baseline setup plus `app-stack`, `k3d-setup`, `opentofu-setup`, and now `security-scanning` — runs start-to-finish on a wiped environment with no manual steps. You'll also rehearse the Demo Day script you'll run live in Week 14.

### Learning Objectives

- Author an idempotent Ansible role that installs real security tooling, following the pattern already used by `app-stack`, `k3d-setup`, and `opentofu-setup`
- Extend `site.yml` in the superset pattern — append, never rewrite — and correctly identify which prior weeks are (and are not) Ansible roles
- Execute and verify a full dry-run rebuild (wipe → playbook → verification) with zero manual intervention
- Rehearse a timed technical demo as a team

### Prerequisites

- Weeks 11-12 complete: `security-scanning.yml` runs SBOM generation (advisory), secrets detection, DAST (advisory), and CRITICAL vulnerability blocking, all proven against real test branches
- `week-11/ansible/roles/security-scanning/` already scaffolded with `tasks/main.yml` (installs Grype and gitleaks via curl, idempotent via `creates:`), `defaults/main.yml`, `vars/main.yml`, and `handlers/main.yml` — Week 13 is about wiring this into the playbook and proving it, not starting from a blank role
- A host or team container you're willing to have Ansible modify (the dry run installs real binaries to `/usr/local/bin`)

### Sprint 7, Part 1

Week 13 is synchronous — get the team together for the Ansible work and, especially, the demo rehearsal, which is much harder to do well asynchronously. Sprint 7 spans Weeks 13-14 and closes out the track.

---

### Part 1: Know What's Actually a Role

Before touching `site.yml`, make sure the team can name the real roles. Only three exist in the Weeks 1-9 baseline: `app-stack` (Week 2, brings up the Docker Compose stack), `k3d-setup` (Week 3, creates the k3d cluster), and `opentofu-setup` (Week 4, installs and initializes OpenTofu). Weeks 6, 7, and 8 are real deliverables but **not** Ansible roles, and don't get "added" the same way:

- **Week 6** is the `.github/workflows/ci.yml` GitHub Actions workflow (build/push image, Trivy CRITICAL scan, load-test gate) — a CI pipeline, not something Ansible installs or reruns.
- **Week 7** is a set of plain `kubectl apply -f manifests/` manifests (NetworkPolicies, hardened SecurityContext, RBAC) applied directly to the cluster — there is no `nginx` or `kompose` role.
- **Week 8** is the `week-8/restic-env.sh` shell script and its runbook for restic backups to MinIO — a script and documentation, not a role.

This track adds one real new role: `security-scanning`.

---

### Part 2: Finish the security-scanning Role

**Step 1.** Review `week-11/ansible/roles/security-scanning/tasks/main.yml`. It already installs Grype and gitleaks via their official install scripts, with `creates:` guards for idempotency and `command`/`failed_when` verification steps — this matches the pattern used by `app-stack` and `opentofu-setup`. If your Week 10 decision was Trivy and/or trufflehog instead, adapt the tasks accordingly; `week-11/ansible/roles/security-scanning/vars/main.yml` already has install URLs stubbed for both.

**Step 2.** Confirm the role's defaults (`week-11/ansible/roles/security-scanning/defaults/main.yml`) point at the real image — `application_image: "week-2-flask:latest"` — not a placeholder.

**Step 3.** Run a syntax check before touching the live environment:

```bash
ansible-playbook -i week-11/ansible/inventory week-11/ansible/site.yml --syntax-check
```

---

### Part 3: Add the Role to site.yml

**Step 1.** Confirm `week-11/ansible/site.yml` already has a play named "DevSecOps - SBOM and Secrets Detection (Challenge Track 3)" that runs the `security-scanning` role, after the baseline setup, `app-stack`, `k3d-setup`, and `opentofu-setup` plays — this is the superset pattern: append, don't rewrite what's already there.

```yaml
- name: DevSecOps - SBOM and Secrets Detection (Challenge Track 3)
  hosts: localhost
  connection: local
  become: yes

  roles:
    - security-scanning
```

**Step 2.** If your role installs additional tools beyond Grype/gitleaks (e.g., Trivy, trufflehog, or ZAP), add the corresponding tasks now rather than leaving them for Week 14.

> **Enterprise Pattern:** The "superset, never rewrite" convention this course has used since Week 2 is exactly how real infrastructure-as-code repos avoid regressions — every play stays runnable independently, and a new team member can read the file top-to-bottom as a history of what the environment does and why.

---

### Part 4: Dry-Run Deployment

**Step 1.** Simulate the wipe:

```bash
docker system prune -a
k3d cluster delete myapp
k3d cluster create myapp --agents 2 --port "8081:80@loadbalancer" --k3s-arg "--disable=traefik@server:0"
```

**Step 2.** Run the full playbook start to finish:

```bash
cd week-11/ansible
ansible-playbook -i inventory site.yml
```

**Step 3.** Confirm the verification command passes:

```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
which gitleaks 2>/dev/null && echo "gitleaks installed" || which trufflehog 2>/dev/null && echo "trufflehog installed" || echo "FAIL"
```

**Step 4.** Run the playbook a second time immediately and confirm it reports no unexpected changes — this is your idempotency check.

---

### Part 5: Demo Rehearsal

**Step 1.** Walk through the full Week 14 script as a team: wipe → playbook rebuild → tool verification → CI/CD pipeline walkthrough (showing `test/clean-build-w12`, `test/secret-block-w12`, and `test/vuln-critical-w12`) → Q&A.

**Step 2.** Time it. Adjust `week-12/demo-plan.md` with real timings and assign who speaks at each phase.

**Step 3.** Identify the most fragile step (usually the playbook rebuild or a network-dependent download) and agree on a fallback if it fails live.

---

### Validation Checks

**QA runs all validation checks.** By end of week, the playbook must be provably idempotent and demo-ready, not just theoretically correct.

#### Validation Check: Role Present and Wired Correctly

QA confirms `week-11/ansible/roles/security-scanning/tasks/main.yml` exists with real (not TODO) tasks, that `week-11/ansible/site.yml` includes it after the three baseline roles without modifying them, and that `--syntax-check` passes.

#### Validation Check: Dry-Run and Idempotency

QA confirms a full playbook run from a wiped environment completes with no errors, the tool verification command passes, and a second consecutive run reports no unexpected changes.

#### Validation Check: Demo Timing

QA confirms the team has rehearsed the full Week 14 script at least once, that it fits the allocated time slot, and that a fallback plan exists for at least one fragile step.

---

### Deliverables

- [ ] `security-scanning` Ansible role installs and verifies the team's chosen tools idempotently
- [ ] `week-11/ansible/site.yml` includes `security-scanning` in the superset pattern, after `app-stack`, `k3d-setup`, `opentofu-setup`
- [ ] Full dry-run (wipe → playbook → verify) completes with zero manual steps
- [ ] Second consecutive playbook run confirms idempotency
- [ ] Demo rehearsal completed and timed, with a documented fallback for the most fragile step
- [ ] `docs/qa-report-13.md` and `docs/sprint-13-retrospective.md` filled in

---

### Closing Out Sprint 7 — Preparing for Week 14

The Scrum Master should confirm the following are ready before Demo Day:

- Final confirmation that all three test branches (`test/clean-build-w12`, `test/secret-block-w12`, `test/vuln-critical-w12`) still reflect correct pass/fail status
- `week-12/demo-plan.md` finalized with real timings and speaker assignments
- Disk space and network connectivity checked on the machine that will run the live demo
- A designated fallback (screenshots or a pre-recorded run) prepared in case the live rebuild fails on stage

---
