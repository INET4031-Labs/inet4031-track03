# Acceptance Criteria - Week 13

**Week:** 13 (Finalization and Dry Run)  
**Track:** DevSecOps  
**Sprint:** 7

## Deliverable: Ansible Integration and Demo Readiness

### 1. Security Scanning Role Authored

- [ ] Ansible role exists: `ansible/roles/security-scanning/tasks/main.yml`
- [ ] Role installs Grype or Trivy (as chosen in Week 10)
- [ ] Role installs gitleaks or trufflehog (as chosen in Week 10)
- [ ] Role uses standard Ansible modules: `get_url`, `unarchive`, `command`, `shell`
- [ ] Each installation step includes a verification command
- [ ] Role is idempotent (can run multiple times without errors or redundant changes)

**Verification:**
```bash
ls -la ansible/roles/security-scanning/tasks/main.yml
ansible-playbook -i ansible/inventory ansible/site.yml --check
```

### 2. Role Added to site.yml (Superset Pattern)

- [ ] `ansible/site.yml` includes the new role in the roles list
- [ ] Role is listed after `backup` (last role from prior weeks) in the sequence
- [ ] No prior roles are rewritten or removed
- [ ] Playbook syntax is valid (no YAML errors)

**Verification:**
```bash
grep "security-scanning" ansible/site.yml
ansible-playbook -i ansible/inventory ansible/site.yml --syntax-check
```

### 3. Dry-Run Deployment: Simulation Phase

- [ ] Container storage is wiped (simulated): `docker system prune -a`
- [ ] Kubernetes cluster is reset (simulated): `k3d cluster delete && k3d cluster create` or equivalent
- [ ] Cluster recovers and is ready for playbook: `kubectl get nodes` shows Ready
- [ ] Environment is clean and matches initial state

**Verification:**
```bash
kubectl get nodes
kubectl get pods -n default
```

### 4. Dry-Run Deployment: Playbook Execution

- [ ] Playbook runs start-to-finish without manual intervention
- [ ] All roles execute successfully (no errors or failures)
- [ ] Playbook completes in reasonable time (under 30 minutes typical)
- [ ] Output shows tasks for security-scanning role completing

**Verification:**
```bash
ansible-playbook -i ansible/inventory ansible/site.yml
# Check final output for all roles including security-scanning
```

### 5. Idempotency Verification

- [ ] Playbook is run a second time immediately after first run
- [ ] Second run shows no changes (or only expected idempotent re-runs)
- [ ] Both security-scanning tools remain installed and functional

**Verification:**
```bash
ansible-playbook -i ansible/inventory ansible/site.yml
# Output should show "changed=0" or minimal changes on second run
```

### 6. Tool Verification Post-Deployment

- [ ] Verification command passes: `which grype && grype --version` or `which trivy && trivy --version`
- [ ] Secrets detector is available: `which gitleaks && gitleaks version` or equivalent
- [ ] Tools can run locally without errors

**Verification:**
```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
which gitleaks 2>/dev/null && echo "gitleaks installed" || which trufflehog 2>/dev/null && echo "trufflehog installed" || echo "FAIL"
```

### 7. Demo Rehearsal Completed

- [ ] Demo script is practiced and timed
- [ ] All team members present their assigned section
- [ ] Live tool verification commands are executed on stage
- [ ] GitHub Actions pipeline is shown (3+ test runs, at least one passing and two failing)
- [ ] Security gates are explained (why they matter, what they block)
- [ ] Demo duration fits in allocated time slot (typically 10-15 minutes)

**Verification:**
- [ ] Team lead certifies rehearsal was completed
- [ ] All sections executed without major stumbles
- [ ] Timing is within acceptable range

### 8. Documentation Complete

- [ ] Environment log includes dry-run execution steps and results
- [ ] Environment log includes demo rehearsal notes and timing
- [ ] Any issues encountered are documented with resolutions
- [ ] Acceptance criteria checklist is complete

**Verification:** All checks in this section are satisfied and team lead signs off.

## Definition of Done for Week 13

- Ansible role is authored, integrated into site.yml, and working
- Dry-run deployment completes without manual steps
- All security tools are installed and functional
- Idempotency is verified (playbook runs cleanly twice)
- Demo rehearsal is complete and team is ready for Demo Day
- Sprint 7 retrospective (through Week 14) is initiated

## Dependencies

This sprint assumes Weeks 10-12 completion:
- Architecture decisions are finalized
- SBOM generation is working in CI
- Secrets detection is blocking merges
- DAST scanning is integrated
- CRITICAL vulnerability blocking is working

## Notes

- Week 13 is synchronous. Expect in-class collaboration time for Ansible work and demo rehearsal.
- No new code changes to the Flask app or CI pipeline in Week 13; focus is on playbook integration.
- Tools are installed locally on the host (or in the Kubernetes environment); they don't need to run inside a container.
- Week 14 will repeat the dry-run in front of an audience (Demo Day).
