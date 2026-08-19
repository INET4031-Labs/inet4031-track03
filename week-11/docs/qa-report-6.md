# QA Report - Sprint 6, Week 11

**Week:** 11  
**Track:** DevSecOps  
**Sprint:** 6  
**QA Lead:** [Name]  
**Date Completed:** [Date]

## Summary

[Brief statement of Week 11 progress: SBOM and secrets detection integration status. Any blockers?]

## Checklist: Week 11 Acceptance Criteria

### SBOM Generation
- [ ] Tool installed and working locally
- [ ] GitHub Actions step added and runs without errors
- [ ] Output visible in CI logs
- [ ] Does not block merge (exit code 0)
- [ ] Runs on every push and PR

### Secrets Detection
- [ ] Tool installed and working locally
- [ ] GitHub Actions step added and runs without errors
- [ ] Blocks merge on secret detection (exit code 1)
- [ ] Passes on clean code (exit code 0)
- [ ] Runs before image build

### Test Case 1: Clean Code
- [ ] Test branch created and pushed
- [ ] CI workflow passes end-to-end
- [ ] Secrets detector passes
- [ ] SBOM generated successfully
- [ ] Workflow run URL documented

### Test Case 2: Secret Commit
- [ ] Test branch created with fake secret
- [ ] CI workflow fails at secrets detection
- [ ] Merge is blocked
- [ ] Workflow run URL documented
- [ ] Error message is clear and actionable

### Documentation
- [ ] Environment log completed
- [ ] Acceptance criteria checklist filled in
- [ ] Blockers documented
- [ ] Team review confirmed

## Issues Found

[List any gaps or concerns from QA review]

**Issue Example:**
- gitleaks is flagging a legitimate API key in test data (FALSE POSITIVE - MEDIUM)
- SBOM is not being saved as artifact (CRITICAL)
- Secrets detection step is not running before build (CRITICAL)

## Test Execution Summary

**Test 1 (Clean Code):**
- Executed: [Date]
- Branch: [Name]
- Result: [PASS | FAIL]
- Duration: [Minutes]
- Evidence: [Link to workflow run]

**Test 2 (Secret Commit):**
- Executed: [Date]
- Branch: [Name]
- Result: [PASS | FAIL]
- Duration: [Minutes]
- Evidence: [Link to workflow run]

## Recommendations

[Suggestions for Week 12 or fixes needed before proceeding:
- Consider tuning secrets detector to reduce false positives
- Add artifact retention policy to GitHub Actions
]

## Sign-Off

- [ ] All Week 11 acceptance criteria met
- [ ] QA Lead Signature: ___________________
- [ ] Date: ___________________

## Notes for Week 12 Handoff

[Any QA focus areas for DAST integration and vulnerability blocking]
