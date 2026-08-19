---
**Week 1-9 Prerequisite**

Weeks 10-14 assume your completed Weeks 1-9 repositories are available as peer directories in `Student Repositories/`. This track's Ansible roles reference your prior work:
- `devsecops` role uses your Flask application from Week 2 (`../week-02/`)
- `devsecops` role uses your infrastructure code from Week 4 (`../week-04/` or `../infrastructure/`)

Your track repo does NOT copy these — it integrates with them. Ensure your Week 1-9 work is complete and accessible before Week 11.

---

# Week 10: Challenge Kickoff and Architecture Decision

## Overview

Week 10 is the synchronous kickoff for the DevSecOps track. Your team will review the challenge goal, decide on architecture and tooling, and prepare the sprint backlog for the core implementation in Weeks 11-12.

## Challenge Goal

Extend CI/CD with automated security checks that gate code merges. By Demo Day, the pipeline must reject merges that introduce:
- CRITICAL image vulnerabilities (detected via SBOM scan)
- Secrets committed to the repository

## Core Decisions This Week

By end of week, your team must document:

1. **SBOM and Vulnerability Scanning Tool**
   - Grype (recommended for lightweight, fast scanning)
   - Trivy (alternative; richer output, slightly heavier)
   - Decide which and why

2. **Secrets Detection Tool**
   - gitleaks (scans Git history)
   - trufflehog (Truffle Security; deeper detection)
   - Decide which and why

3. **DAST (Dynamic Application Security Testing) Tool**
   - OWASP ZAP (open-source, full-featured)
   - Alternative if your architecture dictates
   - Decide on scope and integration point

4. **CI Integration Plan**
   - How do these tools run in your GitHub Actions workflow?
   - Which step blocks a merge? Which are warnings?
   - Where are reports stored and reviewed?

5. **Baseline Scanning**
   - What vulnerabilities or secrets exist in the current codebase?
   - Plan to document findings without immediate action (Week 10 discovery, Week 11+ remediation)

## Deliverables

- [ ] Architecture decision document (why each tool choice)
- [ ] Sprint backlog for Weeks 11-12 (features, tasks, estimated points)
- [ ] Environment status snapshot (current Flask app + PostgreSQL state)
- [ ] Test plan for validation gates (how you'll know the pipeline blocks correctly)

## Week Structure

**Synchronous work** (in-class or scheduled meetings):
- Day 1: Challenge briefing and Q&A
- Day 2-3: Tool evaluation and team decision
- Day 4-5: Backlog refinement and Sprint 5 retrospective closure

## Verification

At end of week:
1. Architecture decision document is complete and team-reviewed
2. Backlog items are defined with acceptance criteria
3. No merges are currently blocked (baseline established)

## Acceptance Criteria

See `docs/acceptance-criteria.md` for the formal acceptance checklist.

## Next Steps (Week 11)

Week 11 is the core build sprint. You will:
- Integrate SBOM generation into CI
- Add secrets detection to the pipeline
- Prepare test cases (malicious image, committed secret) for Week 12
