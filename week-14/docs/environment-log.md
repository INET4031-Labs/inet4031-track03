# Environment Log - Week 14

**Week:** 14 (Demo Day)  
**Track:** DevSecOps  
**Sprint:** 7 (Concluded)  
**Date:** [Date of Demo Day]

## Pre-Demo Environment Status

### Host Machine
- Hostname: [Your host]
- OS: [e.g., Windows 11, Ubuntu 22.04]
- Docker: [Version]
- Kubernetes: [k3d version]
- Disk Space Available: [GB - should be at least 10GB for wipe]

**Command to Check:**
```bash
df -h
```

[Paste output]

### Application Stack (Pre-Wipe)
- Flask App: [Running, version/commit]
- PostgreSQL: [Running, data volume]
- Kubernetes Cluster: [Healthy, nodes ready]
- Security Tools: [Grype/Trivy, gitleaks/trufflehog installed and verified]

**Command to Verify:**
```bash
kubectl get nodes
kubectl get pods -n default
grype --version || trivy --version
gitleaks version || trufflehog version
```

[Paste output]

## Demo Day Execution

### Phase 1: Environment Wipe

**Date/Time:** [Date and time]  
**Host:** [Your host]  
**Audience:** [Number of attendees, audience type]

**Commands Executed:**
```bash
# Show current state
kubectl get pods -n default
docker ps

# Wipe storage
docker system prune -a --volumes

# Reset cluster
k3d cluster delete inet4031
k3d cluster create inet4031
```

**Results:**
- Disk space freed: [GB]
- Cluster reset completed: [Yes/No]
- Status: [SUCCESS | ISSUES]

**Issues Encountered:**
[Any unexpected behavior during wipe?]

**Duration:** [Minutes]

### Phase 2: Playbook Rebuild

**Start Time:** [HH:MM]  
**End Time:** [HH:MM]  
**Total Duration:** [Minutes]

**Command:**
```bash
cd ansible
ansible-playbook -i inventory site.yml
```

**Output Summary:**
[Paste final output or key milestones]

**Roles Executed:**
- [ ] app-stack
- [ ] postgres
- [ ] nginx
- [ ] k3d
- [ ] kompose
- [ ] monitoring
- [ ] backup
- [ ] security-scanning

**Status:** [SUCCESS | ERRORS]

**Errors Encountered (if any):**
[Paste error messages and how they were resolved]

### Phase 3: Tool Verification

**Commands Executed:**
```bash
which grype 2>/dev/null && grype --version || which trivy 2>/dev/null && trivy --version
which gitleaks 2>/dev/null && gitleaks version || which trufflehog 2>/dev/null && trufflehog version
```

**Output:**
```
[Paste tool version outputs]
```

**Status:** [PASS | FAIL]

**Tools Verified:**
- [ ] Grype or Trivy installed and working
- [ ] gitleaks or trufflehog installed and working
- [ ] Versions match expectations

### Phase 4: CI/CD Pipeline Demonstration

**Repository:** [GitHub repository URL]  
**Access:** [Shown live to audience? Screenshots?]

**Test Branches Demonstrated:**
1. **test/clean-build-w12** (or similar)
   - Status: [GREEN, PASSED]
   - All gates passed: SBOM, secrets, vulnerability, DAST
   - Merge allowed

2. **test/secret-block-w12** (or similar)
   - Status: [RED, BLOCKED]
   - Failed at: Secrets detection
   - Merge blocked

3. **test/vuln-critical-w12** (or similar)
   - Status: [RED, BLOCKED]
   - Failed at: Vulnerability scan (CRITICAL CVE)
   - Merge blocked

**Screenshot/Evidence:**
[Paste URLs or descriptions of evidence shown to audience]

### Phase 5: Q&A and Discussion

**Questions Asked:**
[Summarize questions from audience and team responses]

**Key Talking Points Covered:**
- [ ] Tool selection rationale
- [ ] Integration into development workflow
- [ ] Value of shifting security left
- [ ] Handling blocked merges (remediation process)
- [ ] Performance impact on CI pipeline
- [ ] Future enhancements

## Post-Demo Environment Status

### Cluster and Application

**Command:**
```bash
kubectl get nodes
kubectl get pods -n default
docker ps
```

[Paste output]

**Status:** [Application fully operational? Database connected?]

### Storage Snapshot (Post-Demo)

```bash
df -h
docker system df
```

[Paste output]

## Demo Assessment

### Timing
- Total Demo Duration: [Minutes]
- Allocated Time Slot: [Minutes]
- Status: [WITHIN TIME | OVER TIME | UNDER TIME]

### Delivery
- [ ] All phases executed smoothly
- [ ] Team members executed their assigned sections
- [ ] Audience engagement observed
- [ ] No major technical failures

### Technical Execution
- [ ] Playbook ran cleanly
- [ ] Tools verified and working
- [ ] CI/CD pipeline visible and clear
- [ ] No fallbacks needed

## Lessons Learned and Recommendations

[Record team observations on the demo:
- What went well?
- What would you do differently next time?
- Any improvements to the playbook or automation?
]

## Track Completion Summary

**DevSecOps Track Status:** COMPLETE

**Deliverables Achieved:**
1. [X] Week 10: Architecture decisions and backlog planning
2. [X] Week 11: SBOM and secrets detection in CI
3. [X] Week 12: DAST and vulnerability blocking
4. [X] Week 13: Ansible playbook integration and dry-run
5. [X] Week 14: Live demo and environment rebuild

**Tools Integrated:**
- SBOM Tool: [Grype | Trivy]
- Secrets Detector: [gitleaks | trufflehog]
- DAST Tool: [OWASP ZAP]
- Vulnerability Blocking: [Trivy | Grype]
- Automation: [Ansible playbook]

**Security Gates Active:**
- [ ] Secrets detection blocks merge
- [ ] CRITICAL vulnerability detection blocks merge
- [ ] SBOM generation (advisory)
- [ ] DAST scanning (advisory)

## Sign-Off

**Demo Day Completion:** [Date and signatures]

- [ ] Team Lead: ___________________
- [ ] Tech Lead: ___________________
- [ ] QA Lead: ___________________
- [ ] Date: ___________________
