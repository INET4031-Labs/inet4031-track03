# Acceptance Criteria - Week 10

**Week:** 10 (Challenge Kickoff)  
**Track:** DevSecOps  
**Sprint:** 5 (continuation)

## Sprint 5 Closure

This week closes Sprint 5 (which began in Week 9) and prepares Sprint 6. The following must be satisfied to mark Week 10 complete:

### 1. Challenge Architecture Decision (Team-Reviewed)

- [ ] Architecture decision document exists: `week-10/architecture-decision.md`
- [ ] Document includes rationale for SBOM/vulnerability scanner choice
- [ ] Document includes rationale for secrets detection tool choice
- [ ] Document includes rationale for DAST tool choice
- [ ] Document includes CI/CD integration strategy (which steps block, which warn)
- [ ] All team members have reviewed and signed off

**Acceptance:** Team lead certifies the decision document.

### 2. Sprint Backlog for Weeks 11-12

- [ ] Backlog exists: `week-10/backlog.md` or equivalent
- [ ] At least 8 user stories or tasks defined for core implementation
- [ ] Each item has clear acceptance criteria
- [ ] Items are estimated (story points or hours)
- [ ] Work is distributed across team roles (no single person can complete it solo)
- [ ] Dependencies between tasks are documented

**Acceptance:** Tech lead certifies backlog is ready for Weeks 11-12 sprint.

### 3. Environment Status Snapshot

- [ ] Environment log documents the current state of Flask, PostgreSQL, and Kubernetes
- [ ] Cluster is healthy: `kubectl get nodes` shows Ready
- [ ] Application stack is operational
- [ ] Storage usage is documented
- [ ] Any existing CI/CD workflows are listed

**Acceptance:** Ops lead confirms environment is stable for Week 11 work.

### 4. Test Plan for Validation Gates

- [ ] Test plan exists: `week-10/test-plan.md`
- [ ] Test cases include:
  - Pushing a commit with a "fake secret" to a test branch and verifying secrets detector blocks it
  - Pushing a container image with a CRITICAL vulnerability and verifying scanner blocks it
  - Pushing clean code and verifying the pipeline passes
- [ ] Test plan is executable (steps are clear)
- [ ] Success/failure criteria are defined for each test

**Acceptance:** Security engineer or QA lead reviews and approves the test plan.

### 5. Baseline Reconnaissance Report

- [ ] Report documents: `week-10/baseline-findings.md`
- [ ] Current codebase is scanned for secrets (report shows pass or findings)
- [ ] Current container images are scanned for vulnerabilities (report shows results)
- [ ] Any findings are documented (not necessarily remediated in Week 10)
- [ ] This establishes the baseline for improvement in Week 11-12

**Acceptance:** Security engineer reviews baseline report.

## Definition of Done for Week 10

All five items above are complete and team-signed. The sprint retrospective is filled in. Week 11 work can begin immediately without waiting for Week 10 decisions.

## Notes

- Week 10 is synchronous. Expect in-class collaboration time.
- No code changes to the Flask app are required in Week 10; this is planning and decision-making.
- The Ansible playbook is not touched in Week 10 (that comes in Week 13).
