# Environment Log - Week 11

**Week:** 11 (Core Build)  
**Track:** DevSecOps  
**Sprint:** 6 (Core Implementation)  
**Date:** [Date]

## Environment Status at Start of Week

### Host Machine
- Hostname: [Your host]
- OS: [e.g., Windows 11, Ubuntu 22.04]
- Docker: [Version]
- Kubernetes: [k3d version]

### CI/CD Environment
- GitHub Actions: Enabled
- Secrets Configured: [GitHub Actions secrets for registry, etc.]
- Runners: [GitHub-hosted or self-hosted]

### Container Registry
- Registry: [localhost:5000 for k3d, or external]
- Images Available: [flask, postgres, nginx]
- Access: [Confirmed working in CI]

### Application Stack
- Flask App: [Container image available, tag]
- PostgreSQL: [Running, data volume attached]
- Kubernetes: [Pods running, cluster healthy]

**Command to Verify:**
```bash
kubectl get pods -n default
docker images | grep -E 'flask|postgres'
```

## Deployments and Configuration This Week

### SBOM Tool Installation (Local)

**Tool:** [Grype | Trivy]  
**Installation Command:**
```bash
[Paste exact installation command used]
```

**Version Installed:** [Output of `grype --version` or `trivy --version`]

### Secrets Detection Tool Installation (Local)

**Tool:** [gitleaks | trufflehog]  
**Installation Command:**
```bash
[Paste exact installation command used]
```

**Version Installed:** [Output of `gitleaks version` or `trufflehog version`]

### GitHub Actions Workflow Updates

**Workflow File:** `.github/workflows/build.yml` (or equivalent)

**New Steps Added:**
1. SBOM Generation
   - Step name: [e.g., "Generate SBOM"]
   - Runs on: [every push, PR, etc.]
   - Output: [artifact name, log location]

2. Secrets Detection
   - Step name: [e.g., "Detect Secrets"]
   - Runs on: [every push before image build]
   - Exit on failure: [Yes/No]

**Workflow Status:** [Is it running? Any errors in logs?]

## Testing Executions

### Test 1: Clean Code Merge (Passing)

**Test Branch:** [Branch name, e.g., `test/clean-merge`]  
**Workflow Run URL:** [Link to GitHub Actions run]  
**Result:** [PASSED | FAILED]  
**Duration:** [Minutes]

**Verification Steps:**
```bash
git checkout -b test/clean-merge
# Make a valid code change (e.g., add a comment)
git commit -m "Test commit"
git push origin test/clean-merge
# Wait for CI to complete
```

**Observations:**
[Did secrets detector pass? Did SBOM generate?]

### Test 2: Secret Commit (Blocking)

**Test Branch:** [Branch name, e.g., `test/secret-commit`]  
**Workflow Run URL:** [Link to GitHub Actions run]  
**Result:** [BLOCKED | PASSED (unexpected) | FAILED]  
**Duration:** [Minutes]

**Verification Steps:**
```bash
git checkout -b test/secret-commit
# Add a fake secret to a file
echo "aws_secret_access_key=AKIA1234567890ABCDEF" >> config.txt
git add config.txt
git commit -m "Test secret"
git push origin test/secret-commit
# Wait for CI to block
```

**Observations:**
[Did secrets detector catch it? At what step?]

## Storage Snapshot (End of Week)

```bash
df -h
docker system df
```

[Paste output here]

## Known Issues / Blockers

- [ ] Tool installation completed successfully
- [ ] CI workflow syntax is correct
- [ ] Tests executed as planned
- [ ] [Add any other blockers]

## Notes for Week 12

[Dependencies, follow-up items, or environment prep notes]
