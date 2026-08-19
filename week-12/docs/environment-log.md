# Environment Log - Week 12

**Week:** 12 (Integration Testing)  
**Track:** DevSecOps  
**Sprint:** 6 (concluded)  
**Date:** [Date]

## Environment Status at Start of Week

### Host Machine
- Hostname: [Your host]
- OS: [e.g., Windows 11, Ubuntu 22.04]
- Docker: [Version]
- Kubernetes: [k3d version]

### CI/CD Environment (Continued from Week 11)
- GitHub Actions: Enabled
- Secrets Configured: [Registry access, etc.]
- Runners: [GitHub-hosted or self-hosted]

### Container Registry
- Registry: [localhost:5000 for k3d, or external]
- Current Images: [list of images and tags]

### Tools Installed (Inherited from Week 11)
- SBOM Tool: [Grype or Trivy, version]
- Secrets Detector: [gitleaks or trufflehog, version]
- DAST Tool: [OWASP ZAP, version]

## Deployments and Configuration This Week

### DAST Tool Integration

**Tool:** [OWASP ZAP | Other]  
**Installation/Configuration:**
```bash
[Paste installation command or Docker pull]
```

**GitHub Actions Step Added:**
- Step name: [e.g., "Run DAST Scan"]
- Runs after: [deployment to staging, or local test environment]
- Output: [Report format, artifact name]

**Configuration Details:**
- Target URL: [http://localhost:8080 or staging URL]
- Scan Depth: [baseline, full, or specific endpoints]
- Report Format: [HTML, JSON, XML]

### Vulnerability Blocking Logic

**Implementation:**
```yaml
[Paste the exact step that checks for CRITICAL severity vulnerabilities]
```

**Tool Used:** [Trivy with --severity flag | Grype with --fail-on flag]  
**Exit on Failure:** [Yes, exit code 1]  
**Blocks Merge:** [Yes]

## Testing Executions (Week 12)

### Test Case 1: Clean Image + Clean Code (PASS)

**Test Branch:** [Branch name, e.g., `test/week12-clean-build`]  
**Date Executed:** [Date]  
**Workflow Run URL:** [Link to GitHub Actions run]  
**Result:** [PASSED]

**Verification Steps:**
```bash
git checkout -b test/week12-clean-build
# Ensure base image is clean (no CRITICAL CVEs)
# Ensure code has no secrets
git push origin test/week12-clean-build
```

**CI Pipeline Results:**
- SBOM Generation: [PASS]
- Secrets Detection: [PASS]
- Vulnerability Scan (CRITICAL check): [PASS]
- DAST Scan: [PASS]
- Merge Allowed: [YES]

**DAST Report Summary:**
- High Severity Issues: [Number]
- Medium Severity Issues: [Number]
- Low Severity Issues: [Number]
- Recommendations: [Brief summary]

### Test Case 2: CRITICAL Vulnerability (BLOCK)

**Test Branch:** [Branch name, e.g., `test/week12-vuln-block`]  
**Date Executed:** [Date]  
**Workflow Run URL:** [Link to GitHub Actions run]  
**Result:** [BLOCKED]

**Setup:**
```bash
git checkout -b test/week12-vuln-block
# Use a base image with known CRITICAL CVE (e.g., ubuntu:16.04)
# Modify Dockerfile to use vulnerable image
# Push and trigger CI
```

**CI Pipeline Results:**
- SBOM Generation: [PASS]
- Secrets Detection: [PASS]
- Vulnerability Scan (CRITICAL check): [FAIL - CRITICAL found]
- Step that failed: [Trivy or Grype, exit code 1]
- Merge Blocked: [YES]

**Expected Failure Message:**
```
[Paste error output showing CRITICAL vulnerability detected]
```

**CVE Identified:**
- CVE ID: [e.g., CVE-2021-xxxxx]
- Severity: [CRITICAL]
- Affected Package: [Package name and version]

### Test Case 3: Secret Commit (BLOCK) - Reconfirm

**Test Branch:** [Branch name, e.g., `test/week12-secret-recheck`]  
**Date Executed:** [Date]  
**Workflow Run URL:** [Link to GitHub Actions run]  
**Result:** [BLOCKED]

**Verification Steps:**
```bash
git checkout -b test/week12-secret-recheck
echo "password=super_secret_credentials_12345" >> config.txt
git add config.txt
git commit -m "Test"
git push origin test/week12-secret-recheck
```

**CI Pipeline Results:**
- Secrets Detection: [FAIL - secret detected]
- Step that failed: [gitleaks or trufflehog, exit code 1]
- Merge Blocked: [YES]

**Secret Detected:** [Type of secret found, obfuscated]

## Storage Snapshot (End of Week 12)

```bash
df -h
docker system df
```

[Paste output here]

## Known Issues / Blockers

- [ ] DAST tool installed and accessible
- [ ] Vulnerability blocking logic implemented correctly
- [ ] All three test cases executed successfully
- [ ] No false positives or negatives
- [ ] DAST reports are meaningful and actionable

[Add any other issues]

## Sprint 6 Conclusion

**Sprint 6 Dates:** [Week 11 start date] - [Week 12 end date]  
**Deliverables Met:**
- [ ] SBOM generation in CI (Week 11)
- [ ] Secrets detection blocking in CI (Week 11)
- [ ] DAST scanning integrated (Week 12)
- [ ] CRITICAL vulnerability blocking (Week 12)
- [ ] All test cases documented (Week 12)

## Notes for Week 13

[Any dependencies, environment prep, or Ansible integration notes]
