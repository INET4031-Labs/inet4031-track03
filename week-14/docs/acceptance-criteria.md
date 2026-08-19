# Acceptance Criteria - Week 14

**Week:** 14 (Demo Day)  
**Track:** DevSecOps  
**Sprint:** 7 (Conclusion)

## Deliverable: Live Demo and Final Rebuild

### 1. Environment Wipe (Simulated or Real)

- [ ] Disk space available for wipe (at least 10GB recommended)
- [ ] Container storage is wiped: `docker system prune -a --volumes` executed successfully
- [ ] Kubernetes cluster is reset (k3d delete + create, or cluster restart if applicable)
- [ ] Environment is clean and no prior state remains
- [ ] Cluster recovers to Ready state: `kubectl get nodes` shows Ready

**Verification:**
```bash
docker system df  # Should show minimal usage
kubectl get nodes  # Should show Ready
```

### 2. Playbook Rebuild (Live Execution)

- [ ] Playbook runs start-to-finish without manual intervention
- [ ] All eight roles execute in correct order:
  1. app-stack
  2. postgres
  3. nginx
  4. k3d
  5. kompose
  6. monitoring
  7. backup
  8. security-scanning
- [ ] No errors or failures occur during execution
- [ ] Playbook completes in reasonable time (typically 10-15 minutes)
- [ ] Final status shows all tasks completed

**Verification:**
```bash
ansible-playbook -i ansible/inventory ansible/site.yml
# Exit code should be 0
# Output should show all roles completing
```

### 3. Tool Verification (On Live Audience)

- [ ] Grype or Trivy is installed and accessible
- [ ] Version command runs successfully: `grype --version` or `trivy --version`
- [ ] gitleaks or trufflehog is installed and accessible
- [ ] Version command runs successfully: `gitleaks version` or `trufflehog version`
- [ ] Both tools are demonstrated in front of audience
- [ ] Verification output is visible to audience (on screen)

**Verification:**
```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
```

### 4. CI/CD Pipeline Demonstration

- [ ] GitHub Actions pipeline is accessible (live or via screenshot)
- [ ] Workflows tab is visible showing build history
- [ ] At least three test branches are demonstrated:
  - [ ] One passing (clean code, clean image, all gates pass)
  - [ ] One failing on secrets detection
  - [ ] One failing on CRITICAL vulnerability detection
- [ ] Failure messages are visible and explained to audience
- [ ] Team can navigate to each test run and point out key details

**Verification:**
- GitHub repository is accessible
- Test branch URLs can be shared with audience
- CI logs or reports are human-readable

### 5. Security Gates Explanation

- [ ] Team explains why each gate is important
- [ ] Secrets gate is explained: why detecting secrets before merge is critical
- [ ] Vulnerability gate is explained: why CRITICAL CVEs must be addressed before merge
- [ ] DAST gate is explained: why runtime testing provides additional assurance
- [ ] Audience understands the value of these gates in their workflow

**Talking Points (Minimum):**
- [ ] Shift-left security: catching issues early in the pipeline
- [ ] Developer experience: clear error messages help fix issues quickly
- [ ] Risk management: prevents insecure code and data leaks from reaching production
- [ ] Compliance: automated auditing trail of all security checks

### 6. Demo Timing and Flow

- [ ] Total demo duration is within allocated time slot (typically 10-15 minutes)
- [ ] Breakdown:
  - [ ] Wipe and reset: 2-3 minutes
  - [ ] Playbook rebuild: 5-10 minutes (will show live progress)
  - [ ] Tool verification: 1-2 minutes
  - [ ] CI/CD demonstration: 2-3 minutes
  - [ ] Q&A and wrap-up: 1-2 minutes
- [ ] Transitions between phases are smooth
- [ ] All team members have practiced their sections

**Verification:** Timing rehearsal completed successfully in Week 13.

### 7. Q&A Readiness

- [ ] Team is prepared to answer questions about:
  - Tool selection and alternatives considered
  - Integration challenges and how they were overcome
  - Performance impact on CI pipeline
  - How developers handle blocked merges
  - Plans for future improvements
- [ ] Team can provide confident, accurate responses
- [ ] No topic is left unaddressed

**Verification:** Q&A practiced during Week 13 rehearsal.

### 8. Documentation Complete

- [ ] Environment log documents entire demo execution (wipe, rebuild, verification)
- [ ] Environment log includes timing details
- [ ] Environment log documents any issues encountered and how they were resolved
- [ ] Acceptance criteria checklist is complete
- [ ] Sprint 7 retrospective (shared with Week 13) is filled in and signed

**Verification:** All documentation sections are completed by tech lead.

## Definition of Done for Week 14

- Playbook executes cleanly from start to finish in front of audience
- All security tools are installed and verified on live stage
- CI/CD pipeline demonstrates all three gates (pass case and two fail cases)
- Team confidently presents and answers questions
- Demo completes within time slot
- Track is complete and ready for course graduation

## Dependencies

This week assumes all prior weeks are complete:
- Week 10: Architecture decisions finalized
- Week 11: SBOM and secrets detection in CI
- Week 12: DAST and vulnerability blocking verified
- Week 13: Ansible playbook tested and demo rehearsed

## Success Metrics

- [ ] Playbook runs successfully end-to-end on live stage
- [ ] No manual interventions required
- [ ] All security tools present and working
- [ ] Audience understands the value of the security gates
- [ ] Team is confident and articulate in presentation
- [ ] Demo is memorable and compelling

## Notes

- Week 14 is synchronous and culminates in a live presentation.
- This is the capstone of the DevSecOps track and demonstrates mastery of CI/CD security integration.
- The Ansible playbook becomes a permanent part of the course's deployment automation.
- The security gates remain active and protect the development pipeline going forward.
