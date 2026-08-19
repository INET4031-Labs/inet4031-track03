# Environment Log - Week 10

**Week:** 10 (Challenge Kickoff)  
**Track:** DevSecOps  
**Date:** [Date]

## Environment Status at Start of Week

### Host Machine
- Hostname: [Your host]
- OS: [e.g., Windows 11, Ubuntu 22.04]
- Docker: [Version, e.g., Docker Desktop 4.x or Docker Engine 24.x]
- Kubernetes: [k3d version, e.g., v1.27]

### Container Cluster
- Cluster Name: [e.g., inet4031]
- K3d Version: [Version]
- Nodes: [Number of nodes, roles]
- Status: [healthy, degraded, etc.]

**Command to Verify:**
```bash
k3d cluster list
kubectl get nodes
```

### Application Stack
- Flask App: [Running in-container, version/commit]
- PostgreSQL: [Version, data volume attached]
- Nginx: [Version, reverse proxy status]
- Status: [operational, needs restart, etc.]

**Command to Verify:**
```bash
kubectl get pods -n default
kubectl logs -n default -l app=flask --tail=5
```

### Storage Snapshot
```bash
df -h
docker system df
```

[Paste output here]

### Current CI/CD Pipeline
- Repository: [GitHub repo URL]
- Current Workflows: [List any existing workflows]
- Secrets Available: [Are credentials configured for image registry, etc.?]

## Decisions and Deployments This Week

### Tool Selections Finalized

**SBOM/Vulnerability Scanner:** [Tool name and version]  
**Secrets Detector:** [Tool name and version]  
**DAST Tool:** [Tool name and version]  
**CI/CD Platform:** [GitHub Actions details]

### Configuration Changes

[Record any environment variable changes, registry setup, etc.]

## Known Issues / Assumptions

- [ ] DevSecOps tooling is subject to professor review (standard Weeks 10-14 caveat)
- [ ] Current image registry supports scanning: [confirm or flag]
- [ ] GitHub Actions secrets are accessible: [confirm setup]

[Add other assumptions discovered this week]

## Environment Status at End of Week

[Repeat the status checks, noting any changes]

### Disk Usage (df -h)
```
[Paste output]
```

### Container Storage (docker system df)
```
[Paste output]
```

## Notes for Week 11

[Any forward-looking notes on environment prep, delays, or dependencies]
