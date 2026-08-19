# Week 13: Finalization and Ansible Dry Run

## Overview

Week 13 is the synchronous finalization sprint for the DevSecOps track. Your team will integrate the security tools into the Ansible playbook, conduct a dry-run deployment, and rehearse the Demo Day scenario.

## Sprint Focus

**Synchronous work** (in-class or scheduled meetings):
- Ansible playbook integration and testing
- Dry-run container rebuild without manual steps
- Demo rehearsal and walkthrough
- Sprint 7 retrospective and review

## Core Deliverable

By end of Week 13, the Ansible playbook must:

1. **Install DevSecOps Tools**
   - Grype or Trivy (SBOM and vulnerability scanning)
   - gitleaks or trufflehog (secrets detection)
   - Tools run idempotently on every playbook run
   - Tools are available for local scanning (not just CI)

2. **Integration Pattern**
   - Add a new Ansible role: `security-scanning` or similar
   - Role installs tools and configures them
   - Role is added to `ansible/site.yml` in the superset pattern (no rewrites of prior roles)
   - Playbook runs on Week 13 and again on Week 14 with no manual steps required

3. **Dry-Run Deployment**
   - Container is wiped (simulated: `docker system prune -a`)
   - Kubernetes cluster is reset (simulated: `k3d cluster delete && k3d cluster create`)
   - Playbook runs start-to-finish
   - All security tools are installed and functional
   - Verification command passes:
     ```bash
     which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
     ```

4. **Demo Rehearsal**
   - Run verification command live
   - Show that Grype/Trivy works: `grype --version` (or equivalent)
   - Show that secrets detector works: `gitleaks version` (or equivalent)
   - Walk through the CI pipeline in GitHub Actions
   - Show the three test branches with their pass/fail results

## Implementation Guidance

### Ansible Role for Security Scanning

**File:** `ansible/roles/security-scanning/tasks/main.yml`

**Example Structure (Grype):**
```yaml
---
- name: Install Grype SBOM Scanner
  block:
    - name: Download Grype Binary
      get_url:
        url: "https://github.com/anchore/grype/releases/download/v0.65.0/grype_0.65.0_linux_amd64.tar.gz"
        dest: "/tmp/grype.tar.gz"
        checksum: "sha256:..."
    
    - name: Extract and Install Grype
      unarchive:
        src: "/tmp/grype.tar.gz"
        dest: "/usr/local/bin"
        remote_src: yes
    
    - name: Verify Grype Installation
      command: grype version
      register: grype_version
      changed_when: false

- name: Install Secrets Detector (gitleaks)
  block:
    - name: Download gitleaks Binary
      get_url:
        url: "https://github.com/gitleaks/gitleaks/releases/download/v8.18.0/gitleaks-linux-x64"
        dest: "/usr/local/bin/gitleaks"
        mode: '0755'
    
    - name: Verify gitleaks Installation
      command: gitleaks version
      register: gitleaks_version
      changed_when: false
```

### Adding the Role to site.yml

**Pattern (Superset):**
```yaml
---
- hosts: all
  roles:
    - app-stack  # Week 2
    - postgres   # Week 2
    - nginx      # Week 2
    - k3d        # Week 3
    - kompose    # Week 4
    - monitoring # Week 7
    - backup     # Week 8
    - security-scanning  # Week 13 (NEW)
```

## Acceptance Criteria

See `docs/acceptance-criteria.md` for formal requirements.

## Verification Command

```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
```

## Week Structure

**Synchronous work** (in-class or scheduled meetings):
- Day 1: Ansible role authoring and review
- Day 2: Dry-run execution and troubleshooting
- Day 3: Demo rehearsal and refinement
- Day 4-5: Sprint 7 retrospective and Week 14 prep

## Success Metrics

- [ ] Ansible role is idempotent (runs twice with no changes on second run)
- [ ] Dry-run deployment completes without manual intervention
- [ ] Verification command passes after playbook run
- [ ] Demo rehearsal is smooth and repeatable
- [ ] All team members can explain the security gates

## Known Issues / Blockers

[Document any Ansible syntax issues, tool compatibility, or deployment problems]

## Next Steps (Week 14)

Week 14 is Demo Day. You will:
- Wipe the container and cluster completely
- Run the playbook one final time on live audience
- Demonstrate that all security gates work
- Answer questions about the implementation
