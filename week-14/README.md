# Week 14: Demo Day - Final Rebuild and Presentation

## Overview

Week 14 is Demo Day. Your team will execute a final live demonstration where the container and Kubernetes cluster are wiped and rebuilt using the Ansible playbook, proving that all configuration is idempotent and no manual steps are required. This is the capstone of the DevSecOps track.

## Demo Day Scenario

**Live Execution:**
1. Wipe container storage: `docker system prune -a --volumes`
2. Reset Kubernetes cluster (if using k3d): `k3d cluster delete && k3d cluster create`
3. Run Ansible playbook: `ansible-playbook -i ansible/inventory ansible/site.yml`
4. Verify security tools are installed and functional
5. Walk through CI/CD pipeline showing all three security gates
6. Answer questions from audience

**Timing:** 10-15 minutes (typical for demo slot)

## Deliverable

By end of Week 14, you will have:

1. **Live Playbook Execution**
   - Full environment rebuild from clean state
   - No manual steps required
   - All prior week's roles execute correctly
   - Security scanning role installs Grype/Trivy and secrets detector

2. **Verification on Live Audience**
   - Grype or Trivy version check
   - Secrets detector version check
   - Navigate to GitHub Actions and show CI pipeline
   - Point out the three test branches and their outcomes

3. **Security Gates Demonstration**
   - Explain why each gate matters
   - Show the passing build (clean code, clean image)
   - Show the secret commit block (red X)
   - Show the CRITICAL vulnerability block (red X)
   - Discuss the value to the development workflow

4. **Q&A and Wrap-Up**
   - Answer questions about tool choices and integration
   - Discuss any challenges overcome
   - Highlight key learnings

## Pre-Demo Checklist

- [ ] All team members are present and have reviewed their talking points
- [ ] Host machine has sufficient disk space for `docker system prune -a` (frees ~5-10GB typically)
- [ ] Kubernetes cluster can be reset quickly (k3d delete + create typically takes 1-2 minutes)
- [ ] Ansible playbook is on disk and accessible
- [ ] GitHub Actions pipeline is visible (public repo or credentials configured)
- [ ] Network connectivity is stable (for GitHub access if showing live)
- [ ] Timing has been rehearsed; team knows who speaks when

## Live Demo Flow

### Phase 1: Environment Wipe (2-3 minutes)

**Commands:**
```bash
# Show current state
kubectl get pods -n default
docker ps

# Wipe storage
docker system prune -a --volumes

# Reset cluster (k3d example)
k3d cluster delete inet4031
k3d cluster create inet4031
```

**Audience Note:** Explain that this is equivalent to a brand-new deployment; no state or configuration is retained.

### Phase 2: Playbook Rebuild (5-10 minutes)

**Command:**
```bash
cd ansible
ansible-playbook -i inventory site.yml
```

**Live Audience Observation:**
- Playbook runs end-to-end
- All roles execute (app-stack, postgres, nginx, k3d, kompose, monitoring, backup, security-scanning)
- Environment comes up cleanly

**Narration:** Walk the audience through what's happening at each role, emphasizing that no manual commands are needed.

### Phase 3: Tool Verification (1-2 minutes)

**Commands:**
```bash
which grype && grype --version || which trivy && trivy --version
which gitleaks && gitleaks version || which trufflehog && trufflehog version
```

**Audience Note:** Confirm that the security tools are installed and ready to use.

### Phase 4: CI/CD Pipeline Review (2-3 minutes)

**Navigate to GitHub Actions:**
1. Open repository in browser
2. Show Workflows tab
3. Point out the three test branches:
   - `test/clean-build-w12` (green checkmark, all gates passed)
   - `test/secret-block-w12` (red X, secrets detected, merge blocked)
   - `test/vuln-critical-w12` (red X, CRITICAL vulnerability found, merge blocked)
4. Click into each and highlight the failure message

**Audience Note:** Explain how these gates protect the pipeline and prevent risky code from reaching production.

### Phase 5: Q&A and Wrap-Up (1-2 minutes)

**Typical Questions:**
- Why Grype/Trivy over other SBOM tools?
- How does this integrate with the development workflow?
- What happens if a developer encounters a blocked merge?
- How often are the security scans updated?

**Key Messages to Reinforce:**
- Security is integrated into every commit
- Merges are gated by objective security criteria
- Tools are lightweight and fast (CI pipeline is not noticeably slower)
- The same playbook that runs in CI can rebuild the entire environment from scratch

## Success Metrics

- [ ] Playbook runs cleanly from start to finish
- [ ] Environment is fully functional after rebuild
- [ ] All security tools are verified and working
- [ ] CI/CD pipeline demonstration is clear and compelling
- [ ] Team answers questions confidently
- [ ] Demo completes within time slot

## Known Issues / Fallbacks

[Document any known fragile steps or fallbacks:
- If Trivy version is outdated, have backup: `trivy image --version`
- If GitHub is unavailable, have a pre-recorded screenshot of CI pipeline
- If playbook times out, have pre-built state as fallback
]

## Post-Demo Notes

After Demo Day, the track is complete. The Ansible playbook becomes part of the course's standard deployment process, and the security gates remain active in the CI/CD pipeline for future development.

## Course Completion

By end of Week 14, the DevSecOps track has:
- Integrated SBOM generation into CI (Week 11)
- Integrated secrets detection into CI (Week 11)
- Integrated DAST scanning into CI (Week 12)
- Implemented blocking gates for CRITICAL vulnerabilities and secrets (Week 12)
- Automated the entire deployment with Ansible playbook (Week 13-14)
- Demonstrated the solution in front of an audience (Week 14)

The pipeline now rejects insecure code automatically, shifting security left into the development process.
