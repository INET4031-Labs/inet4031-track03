# QA Report - Sprint 7, Week 13

**Week:** 13  
**Track:** DevSecOps  
**Sprint:** 7 (Weeks 13-14)  
**QA Lead:** [Name]  
**Date Completed:** [Date]

## Summary

[Brief statement of Week 13 progress: Ansible integration complete? Dry-run successful? Demo rehearsal ready?]

## Checklist: Week 13 Acceptance Criteria

### Ansible Role Authoring
- [ ] Role directory created: `ansible/roles/security-scanning/`
- [ ] tasks/main.yml written with tool installations
- [ ] Installs Grype/Trivy correctly
- [ ] Installs gitleaks/trufflehog correctly
- [ ] Each step includes verification
- [ ] Role is idempotent

### Playbook Integration
- [ ] Role added to ansible/site.yml
- [ ] Placed after backup role (superset pattern)
- [ ] No prior roles modified or removed
- [ ] YAML syntax is valid

### Dry-Run Simulation Phase
- [ ] Container storage wiped successfully
- [ ] Cluster reset (if applicable)
- [ ] Environment clean and ready

### Playbook Execution
- [ ] First run completes without errors
- [ ] All roles execute, including security-scanning
- [ ] Execution time is reasonable
- [ ] No manual interventions needed

### Idempotency
- [ ] Second playbook run shows no or minimal changes
- [ ] Tools remain installed and functional
- [ ] No spurious failures on re-run

### Tool Verification
- [ ] Grype/Trivy installation verified and working
- [ ] gitleaks/trufflehog installation verified and working
- [ ] Both tools run without errors

### Demo Rehearsal
- [ ] Rehearsal completed on scheduled date
- [ ] All team members participated
- [ ] Tool verification commands executed live
- [ ] GitHub Actions pipeline shown with test branches
- [ ] Security gates explained clearly
- [ ] Timing within acceptable range

### Documentation
- [ ] Environment log filled in with dry-run details
- [ ] Demo rehearsal notes documented
- [ ] Any issues and resolutions recorded
- [ ] Acceptance criteria checklist complete

## Issues Found

[List any gaps or concerns from QA review]

**Issue Example:**
- Role syntax has YAML indentation error on line 15 (CRITICAL)
- Trivy version conflict with existing k3d version (HIGH)
- Demo timing ran 25 minutes (over 15-minute slot) (MEDIUM)

## Dry-Run Execution Details

**Date Executed:** [Date]  
**Host Machine:** [OS, Docker version]  
**Cluster:** [k3d version]

**First Playbook Run:**
- Start Time: [HH:MM]
- End Time: [HH:MM]
- Duration: [Minutes]
- Exit Code: [0 = success]
- Errors: [Any errors encountered]

**Idempotency Check (Second Run):**
- Start Time: [HH:MM]
- End Time: [HH:MM]
- Duration: [Minutes]
- Changes: [Number, should be 0 or very minimal]

**Tool Verification Post-Deployment:**
```bash
grype --version: [Output]
gitleaks version: [Output]
```

## Demo Rehearsal Details

**Date Rehearsed:** [Date]  
**Attendees:** [Team members present]  
**Location:** [In-class / Virtual / Lab]

**Script Sections:**
1. Tool Verification: [Time taken, smooth?]
2. GitHub Actions Demo: [Time taken, pipeline shown?]
3. Test Branch Review: [Time taken, clear explanations?]
4. Security Gates Explanation: [Time taken, Q&A covered?]

**Total Demo Duration:** [Minutes]  
**Target Duration:** [10-15 minutes]  
**Status:** [WITHIN TIME | NEEDS TRIMMING | TOO SHORT]

**Observation Notes:**
[Any rough spots, timing issues, clarity issues to address before Week 14]

## Recommendations

[Suggestions for Demo Day execution:
- Trim demo by 3 minutes (skip detailed DAST output)
- Have backup script if tool version issue reoccurs
- Practice handoff between demo sections
]

## Sign-Off

- [ ] All Week 13 acceptance criteria met
- [ ] Ansible playbook is production-ready for Week 14
- [ ] Demo rehearsal is complete and team is confident
- [ ] QA Lead Signature: ___________________
- [ ] Date: ___________________

## Notes for Week 14 Handoff

[Any last-minute setup, environment prep, or troubleshooting tips for Demo Day]
