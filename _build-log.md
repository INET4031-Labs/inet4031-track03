# Track 03 Build Log: DevSecOps

**Track:** Track 3 - DevSecOps  
**Build Date:** 2026-08-18  
**Status:** SCAFFOLD COMPLETE + PHASE 3B REMEDIATION

## Phase 3B Remediation: Ansible Roles Deployment

**Completion Date:** 2026-08-18  
**Status:** COMPLETE

### Changes Made

1. **Created week-11/ansible/roles/ directory structure**
   - Copied Week 1-4 Ansible role directories into `week-11/ansible/roles/`
   - `week-11/ansible/roles/app-stack/` (from Solved Repositories/week-02/ansible/roles/app-stack/)
   - `week-11/ansible/roles/k3d-setup/` (from Solved Repositories/week-03/ansible/roles/k3d-setup/)
   - `week-11/ansible/roles/opentofu-setup/` (from Solved Repositories/week-04/ansible/roles/opentofu-setup/)

2. **Verified Ansible playbook structure**
   - Confirmed `week-11/ansible/site.yml` references all three baseline roles by name (app-stack, k3d-setup, opentofu-setup)
   - Verified role references are real (not fabricated)
   - Confirmed playbook will not error on role lookup

### Impact

- Track 3 now has executable Ansible playbooks for Weeks 1-4 baseline infrastructure
- Week 11+ playbooks can run without students manually copying role files
- The capstone playbook (site.yml) can rebuild the entire environment from scratch with baseline + OWASP ZAP security scanning role

### Verification

All three role directories are present and contain the required tasks:
- ✓ `week-11/ansible/roles/app-stack/tasks/`
- ✓ `week-11/ansible/roles/k3d-setup/tasks/`
- ✓ `week-11/ansible/roles/opentofu-setup/tasks/`

The site.yml playbook references all three roles in proper sequence before the Track 3 (owasp-zap) role.

---

## Phase 3C Fix: SBOM and Secrets Scanning Focus

**Completion Date:** 2026-08-18  
**Status:** COMPLETE

### Changes Made

1. **Updated week-11/ansible/site.yml**
   - Changed Track 3 role reference from `owasp-zap` to `security-scanning`
   - Updated playbook comment: "Track 3: Add SBOM and secrets detection scanning (security-scanning role)"
   - Updated play name: "DevSecOps - SBOM and Secrets Detection (Challenge Track 3)"

2. **Created security-scanning role scaffold**
   - `week-11/ansible/roles/security-scanning/tasks/main.yml` with TODOs for:
     - Installing Grype or Trivy for SBOM generation
     - Installing gitleaks or trufflehog for secrets detection
     - Configuring scanning in CI/CD pipeline
     - Testing SBOM and secrets detection
   - `week-11/ansible/roles/security-scanning/handlers/main.yml` (restart/update handlers)
   - `week-11/ansible/roles/security-scanning/defaults/main.yml` with tool configuration variables
   - `week-11/ansible/roles/security-scanning/vars/main.yml` with installation URLs and thresholds

3. **Verified week-11/README.md focus**
   - Confirmed week-11/README.md already focuses on SBOM and secrets detection
   - No changes needed to README (already correct)

### Impact

- Track 3 now has a clear focus: SBOM generation (Grype/Trivy) and secrets detection (gitleaks/trufflehog)
- Ansible playbook correctly references the security-scanning role
- Students have a clear scaffold to implement scanning tools
- Role structure follows standard Ansible patterns with defaults, variables, tasks, handlers

**Build Status:** ✓ READY FOR EXECUTION

---

## Phase 3D: Security Scanning Implementation

**Completion Date:** 2026-08-18  
**Status:** COMPLETE

### Changes Made

1. **Implemented security-scanning Ansible Role Tasks**
   - File: `week-11/ansible/roles/security-scanning/tasks/main.yml`
   - Replaced 8 debug TODO tasks with actual implementation:
     - Install Grype for SBOM scanning via curl + official install script
     - Verify Grype installation with `--version` check
     - Install gitleaks for secrets detection via GitHub release download
     - Verify gitleaks installation with `version` check
     - Verify both tools are in PATH with `which` commands
   - All tasks use idempotent patterns:
     - `creates` parameter to skip if binary already exists
     - `changed_when: false` for verification tasks to prevent false state changes
     - Error handling with `failed_when` to catch actual failures
   - Proper shell escaping with `set -e` to fail fast on errors

2. **Created GitHub Actions CI Workflow**
   - File: `.github/workflows/security-scanning.yml` (NEW)
   - Multi-job workflow with proper dependencies:
     - **secrets-detection job:** Runs gitleaks to detect secrets in commit history
     - **sbom-generation job:** Builds container image and runs Grype for SBOM
     - **vulnerability-scan job:** Scans image for CRITICAL vulnerabilities
     - **summary job:** Aggregates results and posts to GitHub Actions summary
   - Week 11-12 escalation support:
     - Week 11-12: All scans run in advisory mode (do not fail)
     - Week 13: Secrets detection becomes blocking (exit code 1)
     - Week 14: Vulnerability detection becomes blocking
   - Artifact storage for compliance:
     - gitleaks-report (advisory text)
     - sbom-report (JSON + table formats)
     - vulnerability-report (JSON + table formats)
   - Proper workflow syntax:
     - Uses `actions/checkout@v4` for code
     - Uses `docker/setup-buildx-action@v3` for builder
     - Uses `docker/build-push-action@v5` for image build
     - Uses `actions/upload-artifact@v3` for reports
     - Uses `actions/download-artifact@v3` for summary

3. **Resolved Container Registry Strategy**
   - Added "Local Dev vs. CI Registry Strategy" section to week-11/README.md
   - Documented three approaches:
     - **Local Development:** `localhost:5000/incident-app:latest` on k3d with Ansible role
     - **GitHub Actions CI:** Build image in workflow, scan from Docker daemon (recommended)
     - **Alternative:** GitHub Container Registry (GHCR) for persistent storage
   - Clear usage examples for each approach
   - Recommendation: Use Docker daemon approach (no registry required)
   - Added same documentation to week-12/README.md for continuity

### Validation Results

1. **Ansible Role YAML Syntax**
   - ✓ Valid YAML structure
   - ✓ Proper indentation and formatting
   - ✓ All variables referenced correctly
   - ✓ Shell commands properly escaped
   - ✓ Idempotent patterns implemented (creates, changed_when, failed_when)

2. **Grype Installation Verification**
   - ✓ curl installation script is official and maintained
   - ✓ Downloads to /usr/local/bin/grype with executable permissions
   - ✓ Version verification command: `grype --version`
   - ✓ Idempotent with `creates: /usr/local/bin/grype`

3. **gitleaks Installation Verification**
   - ✓ Downloads latest release from GitHub API
   - ✓ Parses release tag and constructs proper download URL
   - ✓ Version verification command: `gitleaks version`
   - ✓ Idempotent with `creates: /usr/local/bin/gitleaks`
   - ✓ chmod +x for executable permission

4. **GitHub Actions Workflow Syntax**
   - ✓ Valid GitHub Actions YAML
   - ✓ Proper job dependencies with `needs:` keyword
   - ✓ Correct permissions model (read-only for artifacts)
   - ✓ Proper environment variables and secrets handling
   - ✓ Artifact upload/download with correct paths
   - ✓ Summary generation for GitHub UI integration

### Impact

- Track 3 now has complete infrastructure for Weeks 11-14:
  - Ansible role automatically installs scanning tools
  - GitHub Actions workflow runs on every push/PR
  - Clear registry strategy avoids environment-specific issues
  - Week-by-week escalation documented for progressive enforcement
  - Compliance artifacts preserved for audit trails

- Students can now:
  - Run local Ansible playbook to set up scanning tools
  - Push code to GitHub and see automatic security scans
  - Test both pass and fail scenarios
  - Review scan results in workflow artifacts and logs
  - Progress from advisory mode (Week 11-12) to blocking mode (Week 13-14)

### Files Modified/Created

1. **Modified:** `week-11/ansible/roles/security-scanning/tasks/main.yml`
   - 8 debug tasks → 8 working tasks (install/verify tools)

2. **Created:** `.github/workflows/security-scanning.yml`
   - 4-job workflow with secrets, SBOM, vulnerability, summary

3. **Modified:** `week-11/README.md`
   - Added "Container Registry Strategy" section

4. **Modified:** `week-12/README.md`
   - Added registry strategy continuation note

5. **Modified:** `_build-log.md`
   - Added Phase 3D documentation

**Build Status:** ✓ PHASE 3D COMPLETE - READY FOR STUDENT TESTING
