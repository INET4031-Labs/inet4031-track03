# Acceptance Criteria - Week 11

**Week:** 11 (Core Build - Phase 1)  
**Track:** DevSecOps  
**Sprint:** 6

## Deliverable: SBOM Generation and Secrets Detection in CI

### 1. SBOM Generation Integrated

- [ ] SBOM tool (Grype or Trivy) is installed and configured
- [ ] GitHub Actions workflow includes SBOM generation step
- [ ] Step runs on every push to main and every PR
- [ ] SBOM output is generated without errors
- [ ] SBOM is visible in CI logs or stored as workflow artifact
- [ ] Tool runs after container image is built but before deployment
- [ ] SBOM does not block merge (advisory only, exit code 0)

**Verification:**
```bash
# Push a clean commit
git push origin feature/test
# Navigate to GitHub Actions and verify SBOM step completes
# Examine SBOM output in workflow logs
```

### 2. Secrets Detection Integrated

- [ ] Secrets detection tool (gitleaks or trufflehog) is installed and configured
- [ ] GitHub Actions workflow includes secrets detection step
- [ ] Step runs on every push, before image build
- [ ] Step scans Git history and current commit
- [ ] Step exits with code 1 if secrets are detected (blocking merge)
- [ ] Step exits with code 0 if no secrets are found
- [ ] No false positives on legitimate credentials or tokens (e.g., in documentation or examples that are safe)

**Verification:**
```bash
# Local test: detect should work on clean code
gitleaks detect --source . --exit-code 0 || echo "Pass"

# Local test: detect should fail on a fake secret
echo "password=super_secret_123" >> test.txt
gitleaks detect --source . --exit-code 1 || echo "Caught"
```

### 3. Test Case 1: Clean Code Passes Pipeline

- [ ] Test branch created: `test/clean-merge`
- [ ] Clean commit pushed to test branch
- [ ] Secrets detector runs and passes (exit 0)
- [ ] SBOM is generated successfully
- [ ] CI pipeline completes without blocking
- [ ] Merge to main is allowed (no branch protection blocks)
- [ ] Test results documented: date, time, workflow run URL

**Success Criteria:** CI workflow shows green checkmark, no secrets detected, SBOM artifact present.

### 4. Test Case 2: Secret Commit Blocks Merge

- [ ] Test branch created: `test/secret-commit`
- [ ] Fake secret added to repository (e.g., `aws_secret_key=AKIA...`)
- [ ] Commit pushed to test branch
- [ ] Secrets detector runs and fails (exit 1)
- [ ] CI pipeline displays failure status
- [ ] Merge to main is blocked by branch protection
- [ ] Test results documented: date, time, workflow run URL, screenshot of blocked merge attempt

**Success Criteria:** CI workflow shows red X, secrets detector catches the fake secret and blocks the merge.

### 5. Documentation Complete

- [ ] Environment log is filled in with tool versions, installation steps, and test results
- [ ] Acceptance criteria checklist is complete
- [ ] Any issues or blockers are documented
- [ ] Team has reviewed test results and confirmed tool integration

**Verification:** All checks in this section are satisfied and team lead signs off.

## Definition of Done for Week 11

- SBOM generation works end-to-end in CI
- Secrets detection works end-to-end in CI and blocks merges correctly
- Both test cases (clean pass, secret block) have been executed and documented
- No manual intervention is required for future commits (automation is fully configured)
- Week 11 acceptance criteria are signed off by team lead

## Notes

- Week 11 is asynchronous. No specific class time required.
- Week 12 will add DAST scanning and create a third test case for CRITICAL vulnerabilities.
- The Ansible playbook is not updated in Week 11 (that comes in Week 13).
