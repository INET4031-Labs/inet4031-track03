# Acceptance Criteria - Week 12

**Week:** 12 (Integration Testing)  
**Track:** DevSecOps  
**Sprint:** 6 (Conclusion)

## Deliverable: DAST Scanning and Vulnerability Blocking

### 1. DAST Scanning Implemented

- [ ] DAST tool (OWASP ZAP or chosen in Week 10) is installed and configured
- [ ] GitHub Actions workflow includes DAST scanning step
- [ ] DAST step runs after application deployment or in a test environment
- [ ] DAST scans the Flask application for OWASP Top 10 vulnerabilities
- [ ] DAST produces a human-readable report (HTML, JSON, or equivalent)
- [ ] Report is stored as workflow artifact or accessible in CI logs
- [ ] DAST does not block merge (advisory only)

**Verification:**
```bash
# Trigger a clean build and verify DAST runs without errors
# Check CI logs for DAST report
# Examine report for findings, recommendations
```

### 2. CRITICAL Vulnerability Blocking

- [ ] Vulnerability scan (Grype or Trivy) checks for CRITICAL-severity CVEs
- [ ] Scan is configured to fail (exit code 1) if CRITICAL found
- [ ] Scan passes (exit code 0) if no CRITICAL vulnerabilities
- [ ] Scan blocks merge when CRITICAL exists
- [ ] Step runs after SBOM generation and before merge approval
- [ ] Error message is clear and identifies the vulnerable package/CVE

**Verification:**
```bash
# Test 1: Push clean image, verify scan passes
# Test 2: Push image with CRITICAL CVE, verify scan fails and blocks merge
```

### 3. Test Case 1: Clean Build Passes (All Gates)

- [ ] Test branch created: `test/clean-build-w12`
- [ ] Base image has no CRITICAL vulnerabilities
- [ ] No secrets in commit
- [ ] Workflow executes end-to-end:
  - [ ] SBOM generation: PASS
  - [ ] Secrets detection: PASS
  - [ ] Vulnerability scan (CRITICAL check): PASS
  - [ ] DAST scan: PASS (completes, no blocking failures)
- [ ] Merge to main is allowed (no branch protection blocks)
- [ ] Workflow run URL and evidence documented

**Success Criteria:** All four checks show green. No merge is prevented.

### 4. Test Case 2: CRITICAL Vulnerability Blocks

- [ ] Test branch created: `test/vuln-critical-w12`
- [ ] Base image contains a known CRITICAL vulnerability (e.g., ubuntu:16.04 with old kernel CVE)
- [ ] Code is otherwise clean (no secrets)
- [ ] Workflow executes and fails at vulnerability scan:
  - [ ] SBOM generation: PASS
  - [ ] Secrets detection: PASS
  - [ ] Vulnerability scan (CRITICAL check): FAIL (CVE identified, exit code 1)
  - [ ] Merge is blocked by failure
- [ ] Specific CVE ID is logged in CI output
- [ ] Workflow run URL and evidence documented

**Success Criteria:** Vulnerability scan fails with clear error, merge is blocked, CVE is identified.

### 5. Test Case 3: Secret Commit Blocks (Regression Check)

- [ ] Test branch created: `test/secret-block-w12`
- [ ] Base image is clean (no vulnerabilities)
- [ ] Commit includes a fake secret (e.g., `aws_secret_key=AKIA...`)
- [ ] Workflow executes and fails at secrets detection:
  - [ ] Secrets detection: FAIL (secret detected, exit code 1)
  - [ ] Merge is blocked by failure
- [ ] Error message clearly identifies the detected secret pattern
- [ ] Workflow run URL and evidence documented

**Success Criteria:** Secrets detector fails with clear error, merge is blocked.

### 6. Documentation Complete

- [ ] Environment log includes:
  - [ ] DAST tool version and configuration
  - [ ] All three test case executions with dates and URLs
  - [ ] DAST report summary (findings by severity)
  - [ ] Storage snapshot at end of week
- [ ] Acceptance criteria checklist is complete
- [ ] Any issues or deviations are documented
- [ ] Team has reviewed all test results

**Verification:** All checks in this section are satisfied and tech lead signs off.

## Definition of Done for Week 12

- DAST scanning is fully integrated and produces reports
- CRITICAL vulnerability scanning blocks merges correctly
- All three test cases have been executed and documented
- No false positives, no false negatives
- The security gates (secrets, CRITICAL vulnerabilities) work reliably
- Sprint 6 retrospective is complete and signed

## Dependencies on Week 11

This sprint assumes Week 11 completion:
- SBOM generation is working
- Secrets detection is working and blocking on secrets
- Both are integrated into the GitHub Actions workflow

## Notes

- Week 12 is asynchronous. No specific class time required.
- DAST can be run locally or in CI; if running in CI, ensure test deployment is available.
- Week 13 will add Ansible integration; no playbook updates needed in Week 12.
