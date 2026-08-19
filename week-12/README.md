# Week 12: Integration Testing and DAST

## Overview

Week 12 completes Sprint 6 with automated Dynamic Application Security Testing (DAST). You will add DAST scanning to the pipeline, ensure that CRITICAL image vulnerabilities block merges, and provide evidence that both the vulnerability and secrets gates work correctly.

## Sprint Focus

**Asynchronous work:** Continues from Week 11. Teams integrate DAST scanning and finalize the security gates.

## Deliverable

By end of Week 12, the CI pipeline must:

1. **Perform Automated DAST Scanning**
   - Tool: OWASP ZAP (or chosen in Week 10)
   - Scope: Scan the running Flask application for OWASP Top 10 vulnerabilities
   - Integration: Can run in CI after deployment to a test environment or locally
   - Output: Report with findings, categorized by severity
   - Does not block merge (advisory only)

2. **Block Merges on CRITICAL Image Vulnerabilities**
   - Using Grype or Trivy: parse SBOM output and fail if CRITICAL-severity vulnerabilities exist
   - Alternative: Use Trivy with `--severity=CRITICAL` flag and exit code 1
   - Test: Push a base image with known CRITICAL vulnerability and verify pipeline blocks
   - Must pass on clean images

3. **Confirm Secrets Detection Still Blocks**
   - Verify from Week 11 still works
   - Re-test on Week 12 to confirm no regressions

4. **Test Evidence: Three Test Cases**

   **Test Case 1: Clean Image + Clean Code (PASS)**
   - Flask image with no CRITICAL vulnerabilities
   - No secrets in commit
   - Expected: Pipeline passes, merge allowed

   **Test Case 2: CRITICAL Vulnerability (BLOCK)**
   - Container image with known CRITICAL CVE
   - No secrets, clean code
   - Expected: Pipeline fails at vulnerability scan, merge blocked

   **Test Case 3: Secret Commit (BLOCK)**
   - Clean image, no vulnerabilities
   - Commit includes a fake secret
   - Expected: Pipeline fails at secrets detection, merge blocked

## Implementation Guidance

### DAST with OWASP ZAP

**Quick Example:**
```yaml
- name: Run OWASP ZAP Scan
  run: |
    docker run --rm -v $(pwd):/zap/wrk:rw -t owasp/zap2docker-stable \
      zap-baseline.py -t http://localhost:8080 -r dast-report.html
```

### Vulnerability Blocking with Trivy

**Example:**
```yaml
- name: Scan Image for CRITICAL Vulnerabilities
  run: |
    trivy image --severity=CRITICAL --exit-code 1 localhost:5000/incident-app:latest
```

**Example with Grype:**
```yaml
- name: Scan Image for CRITICAL Vulnerabilities
  run: |
    grype localhost:5000/incident-app:latest --fail-on=critical
```

## Container Registry Strategy (Continued from Week 11)

Week 12 uses the same registry approach as Week 11:

- **Local Dev:** `localhost:5000/incident-app:latest` on k3d cluster
- **CI:** Build image in workflow, scan from Docker daemon (no registry push)

This ensures consistency between:
1. Local Ansible playbook execution (Week 11 scan)
2. GitHub Actions CI workflow execution (Weeks 11-12 scan)
3. Week 13 automated deployment with security scanning

No changes to registry strategy needed in Week 12 — focus remains on:
- Adding DAST scanning
- Making vulnerability detection blocking
- Validating all three test scenarios

## Acceptance Criteria

See `docs/acceptance-criteria.md` for formal requirements.

## Verification

All three test cases must execute successfully and be documented:
1. Pipeline passes on clean build
2. Pipeline blocks on CRITICAL vulnerability
3. Pipeline blocks on detected secret

## Sprint Structure

**Week 12 is asynchronous.** Continue the asynchronous pattern from Week 11:
- Daily async standups
- Pair programming as needed
- Code review on CI changes
- Thorough test execution and documentation

## Success Metrics

- [ ] DAST scan runs without errors in CI
- [ ] DAST results are human-readable (HTML or JSON report)
- [ ] Vulnerability scan blocks merge if CRITICAL found
- [ ] Secrets detection still blocks merge
- [ ] All three test cases pass and are documented with screenshots or logs
- [ ] No false positives, no false negatives

## Known Issues / Blockers

[Document any tool compatibility issues, environment issues, or test failures]

## Next Steps (Week 13)

Week 13 is the finalization sprint. You will:
- Review and fix any pipeline issues from Weeks 11-12
- Run a dry-run Ansible deployment to verify the playbook
- Rehearse the Demo Day scenario
- Prepare a summary of what the pipeline gates do

## Note on Ansible Integration

The Ansible playbook will be updated in Week 13 to install the DevSecOps tools locally (grype/trivy and secrets detector). Week 12 focuses on CI integration only.
