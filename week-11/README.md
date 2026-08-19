---
**Week 1-9 Prerequisite**

Weeks 10-14 assume your completed Weeks 1-9 repositories are available as peer directories in `Student Repositories/`. This track's Ansible roles reference your prior work:
- `devsecops` role uses your Flask application from Week 2 (`../week-02/`)
- `devsecops` role uses your infrastructure code from Week 4 (`../week-04/` or `../infrastructure/`)

Your track repo does NOT copy these — it integrates with them. Ensure your Week 1-9 work is complete and accessible before Week 11.

---

# Week 11: Core Build - SBOM and Secrets Detection

## Overview

Week 11 is the core implementation sprint for the DevSecOps track. Your team will integrate SBOM generation and secrets detection into the GitHub Actions CI pipeline, building on the architecture decisions from Week 10.

## Sprint Focus

**Asynchronous work:** Teams self-organize around CI/CD integration tasks. Expected work includes:
- Adding SBOM generation to the build workflow
- Adding secrets detection (gitleaks or trufflehog) as a gating step
- Creating test branches and validating that detection works
- Documenting the pipeline integration

## Deliverable

By end of Week 11, the CI pipeline must:

1. **Generate SBOM on Every Build**
   - Tool: Grype or Trivy (chosen in Week 10)
   - Scope: Scan the Flask application container image
   - Output: SBOM artifact stored in CI or uploaded to workflow logs
   - Must not block the merge (advisory only)

2. **Detect Secrets on Every Commit**
   - Tool: gitleaks or trufflehog (chosen in Week 10)
   - Scope: Scan Git history and current commit for secrets
   - Integration: Runs before image build
   - Result: Blocks merge if secrets are detected
   - Must pass on clean code (no false negatives on real secrets)

3. **Test Coverage**
   - [ ] Create a test branch and commit a fake secret (e.g., "aws_secret_key=AKIA...")
   - [ ] Verify the CI pipeline rejects the commit
   - [ ] Create another test branch and push clean code
   - [ ] Verify the pipeline passes
   - [ ] Document both runs in the environment log

## Implementation Guidance

### SBOM Generation

**Grype Example:**
```yaml
- name: Generate SBOM
  run: |
    grype localhost:5000/incident-app:latest -o table > sbom.txt
    cat sbom.txt
```

**Trivy Example:**
```yaml
- name: Generate SBOM
  run: |
    trivy image localhost:5000/incident-app:latest --format=table > sbom.txt
    cat sbom.txt
```

### Secrets Detection

**gitleaks Example:**
```yaml
- name: Detect Secrets
  run: |
    gitleaks detect --source . --exit-code 1
```

**trufflehog Example:**
```yaml
- name: Detect Secrets
  run: |
    trufflehog git file://. --fail
```

## Acceptance Criteria

See `docs/acceptance-criteria.md` for the formal requirements.

## Verification

Run the verification command to confirm tools are installed:
```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
```

## Container Registry Strategy

### Local Development vs. CI Environment

The security scanning pipeline works differently in local development and CI:

#### Local Development (Ansible Role)
- **Registry:** `localhost:5000` (k3d local registry)
- **Prerequisite:** k3d cluster running locally with `docker-compose.yml`
- **Usage:** 
  ```bash
  # Start k3d with local registry
  docker-compose up -d
  
  # Build and push Flask app to local registry
  docker build -t localhost:5000/incident-app:latest .
  docker push localhost:5000/incident-app:latest
  
  # Run Grype scan against local image
  ansible-playbook -i inventory week-11/ansible/site.yml
  ```
- **Advantages:** 
  - No external dependencies
  - Fast iteration cycles
  - Mirrors CI environment exactly
  - Useful for debugging security issues locally

#### GitHub Actions CI (Workflow)
- **Registry:** Docker daemon on `ubuntu-latest` runner
- **Approach:** Build image in workflow, scan from Docker daemon (no push to registry)
- **Usage:**
  ```yaml
  - name: Build container image
    uses: docker/build-push-action@v5
    with:
      push: false
      load: true
      tags: localhost:5000/incident-app:latest
  
  - name: Scan image
    run: grype localhost:5000/incident-app:latest -o table
  ```
- **Advantages:**
  - No registry required (avoids setup overhead)
  - Consistent with local dev environment
  - Faster CI/CD pipeline
  - Container image never leaves the builder

#### Alternative: GitHub Container Registry (GHCR)
- **Registry:** `ghcr.io/yourorg/incident-app:latest`
- **When to use:** If you need persistent image storage or cross-platform builds
- **Trade-off:** Requires additional setup and auth, slightly slower CI

### Recommended Approach for Week 11

**Use the Docker daemon approach (CI only builds, no push):**
- Simplest to implement
- No external dependencies
- Works on all runners (Ubuntu, macOS, Windows)
- Aligns with Ansible role's `localhost:5000` scanning interface

If students want to test locally on k3d:
1. Build image locally: `docker build -t localhost:5000/incident-app:latest .`
2. Push to k3d: `docker push localhost:5000/incident-app:latest` (if k3d running)
3. Run Ansible playbook to test security scanning tools
4. CI pipeline will build its own image and scan independently

## Sprint Structure

**Week 11 is asynchronous.** Teams work on their own schedule with:
- Daily async standups (Slack or email)
- Pair programming sessions as needed
- Code review on CI workflow changes
- Testing and validation in parallel

## Success Metrics

- [ ] SBOM is generated on every commit
- [ ] SBOM is visible in CI logs or as an artifact
- [ ] Secrets detector blocks a merge when secrets are present
- [ ] Secrets detector passes when code is clean
- [ ] Both test runs are documented with screenshots or logs

## Known Issues / Blockers

[Document any access issues, tool installation failures, or environment problems here]

## Next Steps (Week 12)

Week 12 will add automated DAST scanning and ensure that the pipeline blocks not just secrets but also CRITICAL image vulnerabilities. You'll also prepare evidence of both blocks firing.
