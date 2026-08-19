# Demo Plan: DevSecOps Track

**Demo Date:** [Insert date]  
**Demo Duration:** [XX minutes]  
**Audience:** [Faculty, peers, stakeholders]  
**Location/Format:** [In-person / Virtual / Hybrid]

---

## 1. Demo Sequence

**Step-by-step walkthrough with timing:**

| Time | Step | Description | Owner |
|------|------|-------------|-------|
| 0:00 | Introduction | Brief overview of track objectives | [Name] |
| 0:30 | Component 1 | [Description with specific actions] | [Name] |
| 2:00 | Component 2 | [Description with specific actions] | [Name] |
| 4:00 | Component 3 | [Description with specific actions] | [Name] |
| 5:30 | Results/Analysis | [Description of outputs demonstrated] | [Name] |
| 6:30 | Q&A | Questions from audience | All |

**Total: XX minutes**

### Detailed Walkthrough

**TODO:** Provide step-by-step instructions for each demo component
- Which CI/CD pipelines to show?
- What security checks to demonstrate?
- What containers/deployments to display?
- Where are potential failure points?

---

## 2. Success Criteria

**What must work for the demo to succeed:**

- [ ] CI/CD pipeline triggers and runs successfully
- [ ] Security scanning tools execute without errors
- [ ] Code quality reports generate correctly
- [ ] Container images build and deploy as expected
- [ ] All integrations function properly
- [ ] Network/connectivity is stable (if applicable)
- [ ] Audio/video working (if virtual)
- [ ] Timing is within target duration
- [ ] [Additional track-specific criteria]

---

## 3. Risk Assessment

**What could go wrong:**

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| CI/CD pipeline fails mid-demo | Medium | High | Pre-run pipeline; have cached artifacts |
| Security scan takes too long | Medium | High | Use pre-scanned results; cache dependencies |
| Container registry unavailable | Low | High | Use local images; mock registry if needed |
| Network connectivity issues | Medium | High | Pre-test all connections; have offline backup |
| Code changes break pipeline | Medium | Medium | Revert to known-good commit; have backup |
| Dashboard/metrics slow to load | Low | Medium | Pre-load data; use screenshots if needed |

---

## 4. Recovery Plans

**How to handle each risk:**

### Pipeline Failure
- **Plan A:** Review logs and troubleshoot quickly
- **Plan B:** Skip to next stage; use pre-generated artifacts
- **Plan C:** Play pre-recorded successful pipeline run

### Slow Security Scans
- **Plan A:** Explain what's happening; show results from cache
- **Plan B:** Skip to pre-generated scan results
- **Plan C:** Show screenshots of typical scan output

### Container Issues
- **Plan A:** Use local pre-built images
- **Plan B:** Use Docker Hub cached images
- **Plan C:** Show Dockerfile and explain process verbally

### Network Failure
- **Plan A:** Switch to alternative network
- **Plan B:** Demonstrate with pre-recorded video
- **Plan C:** Show screenshots and architecture diagrams

---

## 5. Key Talking Points

**What to explain to the audience:**

- **DevSecOps Philosophy:** Security integrated into every stage
- **Pipeline Architecture:** How CI/CD integrates security checks
- **Key Security Controls:** What gates prevent bad code from shipping
- **Automation Benefits:** Speed, consistency, early defect detection
- **Audit Trail:** How we maintain compliance and traceability
- **Lessons Learned:** Challenges and successes
- **Future Roadmap:** Planned enhancements

### Sample Talking Points
- [Point 1 - TODO: Fill in]
- [Point 2 - TODO: Fill in]
- [Point 3 - TODO: Fill in]

---

## 6. Contingencies

**Backup plan if primary demo fails:**

### Pre-recorded Pipeline Run
- Location: `week-12/backup/pipeline-recording.mp4`
- Duration: [XX minutes]
- Setup instructions: [Specify how to play]

### Screenshot Fallback
- Location: `week-12/backup/screenshots/`
- Contents:
  - [ ] Screenshot 1: [Pipeline dashboard]
  - [ ] Screenshot 2: [Security scan results]
  - [ ] Screenshot 3: [Deployment status]

### Printed Handout
- Location: `week-12/backup/demo-handout.pdf`
- Contents: Pipeline diagram, security checklist, deployment topology

### Live Code Walk-Through Alternative
- **Scenario:** If systems unavailable, team can:
  1. Walk through pipeline YAML configuration
  2. Explain security scanning strategy verbally
  3. Show container Dockerfile and image hierarchy
  4. Discuss deployment process and rollback procedures

---

## Pre-Demo Checklist

**Day Before:**
- [ ] Test entire pipeline end-to-end
- [ ] Verify all systems accessible (GitHub, registry, deployment)
- [ ] Check network/internet connectivity
- [ ] Clear old artifacts; refresh test data
- [ ] Pre-run pipeline to cache builds
- [ ] Record backup video (if using)
- [ ] Prepare printed handouts
- [ ] Brief all team members on their roles
- [ ] Run through demo once, timing it

**Morning Of:**
- [ ] Verify all services up and healthy
- [ ] Clear caches/reset to clean state
- [ ] Open all relevant browser tabs/terminals
- [ ] Test microphone/audio (if virtual)
- [ ] Verify projection/screen sharing
- [ ] Have backup devices/networks ready
- [ ] Review talking points one more time

---

## Demo Success Log

**After demo, complete the following:**

- [ ] Demo completed successfully?
  - [ ] Yes - Note what worked well
  - [ ] Partial - Note what failed and recovery used
  - [ ] No - Document what went wrong

- [ ] Audience feedback: [Brief notes]
- [ ] Lessons learned: [What to improve next time]
- [ ] Follow-up actions: [Any questions to address]

---

**Demo Rehearsal Completed:** [Date]  
**Final Approval:** [Name/Date]
