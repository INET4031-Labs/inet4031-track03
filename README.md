# Track 3: DevSecOps (Weeks 10-14)

## Overview

This track extends the CI/CD pipeline with automated security checks that gate code merges. You will integrate software bill of materials (SBOM) generation, secrets detection, and dynamic application security testing (DAST) into the development workflow.

## Course Status Caveat

Weeks 10-14 were flagged in the source lab directions as needing re-evaluation with the professor before being finalized. The track structure and overall approach are correct as written; specific deliverables and integration points may still change. Treat this framework as a proposal, not a finished deliverable.

## Architecture Assumptions

This track assumes the same architecture as Weeks 1-9:
- A containerized incident management application (Flask) backed by PostgreSQL.
- A k3d Kubernetes cluster for orchestration.
- An Ansible playbook that can rebuild the environment idempotently.

## Track Goal

Extend CI/CD with automated security checks that gate merges.

## Week Breakdown

- **Week 10:** Challenge kickoff, architecture decision, backlog
- **Week 11:** Core build: implement SBOM generation and secrets-detection as CI steps
- **Week 12:** Challenge build continued: automated DAST scan, evidence that pipeline blocks on vulnerabilities and detected secrets
- **Week 13:** Finalize, Ansible dry run, demo rehearsal
- **Week 14:** Demo Day: container wipe and playbook rebuild

## Directory Structure

```
track-03-devsecops/
├── README.md (this file)
├── week-10/
│   ├── README.md
│   └── docs/
│       ├── sprint-5-retrospective.md
│       ├── environment-log.md
│       ├── acceptance-criteria.md
│       └── qa-report-5.md
├── week-11/
│   ├── README.md
│   └── docs/
│       ├── sprint-6-retrospective.md
│       ├── environment-log.md
│       ├── acceptance-criteria.md
│       └── qa-report-6.md
├── week-12/
│   ├── README.md
│   └── docs/
│       ├── sprint-6-retrospective.md
│       ├── environment-log.md
│       ├── acceptance-criteria.md
│       └── qa-report-6.md
├── week-13/
│   ├── README.md
│   └── docs/
│       ├── sprint-7-retrospective.md
│       ├── environment-log.md
│       ├── acceptance-criteria.md
│       └── qa-report-7.md
└── week-14/
    ├── README.md
    └── docs/
        ├── sprint-7-retrospective.md
        ├── environment-log.md
        ├── acceptance-criteria.md
        └── qa-report-7.md
```

## Deliverables Summary

### Week 11
SBOM generation and secrets-detection added as CI steps.

### Week 12
Automated DAST scan with documented results; pipeline that blocks a merge on a CRITICAL image vulnerability or a detected secret; evidence both blocks fire on a test branch.

### Weeks 13-14
Finalize and prepare for Demo Day; Ansible playbook includes the DevSecOps tooling.

## Verification Command

Run this to verify the DevSecOps tools are installed locally:

```bash
which grype 2>/dev/null && echo "grype installed" || which trivy 2>/dev/null && echo "trivy installed" || echo "FAIL"
```

## Team Roles

Each sprint requires cross-functional input:
- **DevOps Engineer:** CI/CD pipeline configuration, tooling integration
- **Security Engineer:** Vulnerability scanning, policy definition, DAST strategy
- **Software Engineer:** Application integration, test-case design
- **Data Steward / Documentation Lead:** Compliance reporting, runbooks

Refer to `Documents/Sprint_Structure_Layout.md` for role-specific responsibilities.

---

**Track Lead Email:** vangwendy.05@gmail.com
**Last Updated:** 2026-08-18
