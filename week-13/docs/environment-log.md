# Environment Log - Week 13

**Week:** 13 (Finalization)  
**Track:** DevSecOps  
**Sprint:** 7  
**Date:** [Date]

## Environment Status at Start of Week

### Host Machine
- Hostname: [Your host]
- OS: [e.g., Windows 11, Ubuntu 22.04]
- Docker: [Version]
- Kubernetes: [k3d version]

### Application Stack Status
- Flask App: [Running, version/commit]
- PostgreSQL: [Running, data volume]
- Kubernetes Cluster: [Healthy, nodes ready]

**Command to Verify:**
```bash
kubectl get nodes
kubectl get pods -n default
```

### CI/CD and Security Tools (From Weeks 11-12)
- GitHub Actions Workflows: [All passing]
- SBOM Tool: [Grype/Trivy, version]
- Secrets Detector: [gitleaks/trufflehog, version]
- DAST Tool: [OWASP ZAP, version]
- Status: [All gates working correctly]

## Ansible Playbook Development

### New Role Created

**Role Name:** `security-scanning`  
**Location:** `ansible/roles/security-scanning/`  
**Tasks Implemented:**
1. [ ] Install Grype/Trivy
2. [ ] Install gitleaks/trufflehog
3. [ ] Verify installations with command checks
4. [ ] Configure for idempotent runs

**Playbook Integration:**
**File:** `ansible/site.yml`

**Before (Week 12):**
```yaml
roles:
  - app-stack
  - postgres
  - nginx
  - k3d
  - kompose
  - monitoring
  - backup
```

**After (Week 13):**
```yaml
roles:
  - app-stack
  - postgres
  - nginx
  - k3d
  - kompose
  - monitoring
  - backup
  - security-scanning
```

## Dry-Run Execution

### Pre-Dry-Run Environment Snapshot

**Storage Usage:**
```bash
df -h
docker system df
```

[Paste output]

### Dry-Run Steps

**Step 1: Container Wipe Simulation**
```bash
docker system prune -a --volumes
```

**Output:**
[Paste results]

**Step 2: Cluster Reset Simulation (optional if using k3d)**
```bash
k3d cluster delete inet4031
k3d cluster create inet4031
```

**Output:**
[Paste results]

**Step 3: Ansible Playbook Execution (First Run)**

**Command:**
```bash
cd ansible
ansible-playbook -i inventory site.yml
```

**Execution Time:** [Minutes]  
**Completion Status:** [SUCCESS | ERRORS]

**Output Summary:**
[Paste final status or error messages]

**Verification Command Result:**
```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
```

[Paste output: SUCCESS or FAIL]

### Dry-Run Idempotency Check (Second Playbook Run)

**Command:**
```bash
ansible-playbook -i inventory site.yml
```

**Execution Time:** [Minutes]  
**Status:** [No changes expected, or list what changed]

**Verification:**
```bash
grype version
gitleaks version
```

[Paste output]

## Demo Rehearsal

### Timing and Flow

**Date Rehearsed:** [Date]  
**Start Time:** [HH:MM]  
**End Time:** [HH:MM]  
**Total Duration:** [Minutes]

### Demo Script

1. **Verify Tools Installed:**
   ```bash
   which grype && grype --version
   which gitleaks && gitleaks version
   ```
   **Result:** [Output showing versions]

2. **Show GitHub Actions Pipeline:**
   - Navigate to repository
   - Show Workflows tab
   - Click on a passing run
   - Show SBOM output
   - Show secrets detection passed
   - Show vulnerability scan
   - Show DAST results

3. **Show Failing Test Branches:**
   - Navigate to test/secret-block-w12
   - Show workflow failed at secrets detection
   - Navigate to test/vuln-critical-w12
   - Show workflow failed at vulnerability scan

4. **Explain the Security Gates:**
   - Q&A on why each gate is important
   - Discuss remediation process

### Demo Observations

[Notes on smooth vs. rough spots, timing issues, anything to adjust]

### Team Preparedness

- [ ] All members understand their assigned talking points
- [ ] Timing fits within expected Demo Day slot (typically 10-15 min)
- [ ] Fallback plan if a tool is not available (alternative command)

## Storage Snapshot (End of Week 13)

```bash
df -h
docker system df
```

[Paste output]

## Known Issues / Resolved

- [ ] All Ansible syntax errors resolved
- [ ] Dry-run completed successfully
- [ ] Tools installed and verified
- [ ] Demo rehearsal completed

[List any issues and how they were resolved]

## Notes for Week 14

[Anything to be aware of for Demo Day execution, special setup, or last-minute fixes]
