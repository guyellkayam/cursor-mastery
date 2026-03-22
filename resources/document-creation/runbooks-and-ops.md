# Runbooks, Incident Response & Ops Documents

> **TL;DR**: Templates for operational documentation — runbooks for on-call, incident response playbooks, RCA templates, and SLO/SLA definitions.

## Runbook Template

```markdown
# Runbook: [Service/Alert Name]

## Alert
**Alert Name**: [alert name from monitoring]
**Severity**: Critical | Warning | Info
**Service**: [service name]
**Dashboard**: [link to relevant Grafana dashboard]

## Symptoms
[What does the user/operator see?]
- [symptom 1]
- [symptom 2]

## Diagnosis

### Step 1: Check service health
```bash
kubectl get pods -n [namespace] -l app=[service]
kubectl logs -n [namespace] -l app=[service] --tail=50
```

### Step 2: Check dependencies
```bash
# Database
kubectl exec -n [namespace] [pod] -- pg_isready
# Redis
kubectl exec -n [namespace] [pod] -- redis-cli ping
```

### Step 3: Check metrics
- [Dashboard link for request rate]
- [Dashboard link for error rate]
- [Dashboard link for latency]

## Resolution

### Common Fix 1: [description]
```bash
[commands to fix]
```

### Common Fix 2: [description]
```bash
[commands to fix]
```

### Escalation
If unresolved after 15 minutes:
1. Page [team/person] via [PagerDuty/Slack]
2. Create incident in [incident management tool]
3. Start bridge call: [call link]

## Prevention
[What changes would prevent this from happening again?]
```

---

## Incident Response Playbook

```markdown
# Incident Response Playbook

## Severity Levels
| Level | Description | Response Time | Example |
|-------|------------|---------------|---------|
| SEV-1 | Complete outage, data loss risk | 15 min | Production down |
| SEV-2 | Major degradation, workaround exists | 30 min | Slow responses |
| SEV-3 | Minor issue, limited impact | 4 hours | Single feature broken |
| SEV-4 | Cosmetic, no user impact | Next sprint | UI glitch |

## Incident Workflow

### 1. Detect & Declare
- [ ] Alert received from monitoring
- [ ] Assess severity level
- [ ] Create incident channel: #inc-[YYYY-MM-DD]-[short-name]
- [ ] Assign Incident Commander (IC)

### 2. Triage (first 15 min)
- [ ] IC confirms severity
- [ ] Identify affected services
- [ ] Check recent deployments: `git log --oneline --since="2 hours ago"`
- [ ] Check recent config changes
- [ ] Notify stakeholders

### 3. Mitigate
- [ ] Rollback if recent deployment caused it
- [ ] Apply workaround if available
- [ ] Scale resources if load-related
- [ ] Update status page

### 4. Resolve
- [ ] Root cause identified
- [ ] Fix deployed and verified
- [ ] All monitoring green
- [ ] Status page updated: resolved

### 5. Post-Incident
- [ ] Schedule RCA within 48 hours
- [ ] Document timeline
- [ ] Identify action items
- [ ] Update runbooks with new knowledge
```

---

## Root Cause Analysis (RCA) Template

```markdown
# RCA: [Incident Title]

**Date**: [YYYY-MM-DD]
**Duration**: [X hours Y minutes]
**Severity**: SEV-[1/2/3]
**Incident Commander**: [name]
**Author**: [name]

## Summary
[2-3 sentences: what happened, impact, resolution]

## Impact
- **Users affected**: [number or percentage]
- **Duration**: [start time] to [end time] (UTC)
- **Services affected**: [list]
- **Revenue impact**: [if applicable]

## Timeline (UTC)
| Time | Event |
|------|-------|
| HH:MM | [first indicator] |
| HH:MM | [alert fired] |
| HH:MM | [IC assigned] |
| HH:MM | [root cause identified] |
| HH:MM | [fix deployed] |
| HH:MM | [monitoring confirmed resolution] |

## Root Cause
[Detailed technical explanation of what went wrong and why]

## Contributing Factors
1. [factor that made the incident worse or harder to diagnose]
2. [factor]

## What Went Well
- [effective response aspect]
- [tool or process that helped]

## What Went Poorly
- [delayed detection or response]
- [missing runbook or documentation]

## Action Items
| Action | Owner | Priority | Due Date | Status |
|--------|-------|----------|----------|--------|
| [action] | [name] | P1 | [date] | Open |
| [action] | [name] | P2 | [date] | Open |

## Lessons Learned
[Key takeaways for the team]
```

---

## SLO/SLA Definitions

```markdown
# Service Level Objectives

## [Service Name]

### Availability SLO
- **Target**: 99.9% (43.8 min downtime/month)
- **Measurement**: Successful requests / Total requests
- **Window**: 30-day rolling
- **Exclusions**: Planned maintenance (announced 48h in advance)

### Latency SLO
- **Target**: p99 < 500ms, p50 < 100ms
- **Measurement**: Server-side request duration
- **Window**: 30-day rolling

### Error Rate SLO
- **Target**: < 0.1% 5xx error rate
- **Measurement**: 5xx responses / Total responses
- **Window**: 30-day rolling

### Error Budget
- **Monthly budget**: 0.1% = 43.2 minutes of downtime
- **Remaining**: [calculated from monitoring]
- **Policy**: When budget < 25%, freeze non-critical deployments

### SLA (External)
| Tier | Availability | Support Response | Credits |
|------|-------------|-----------------|---------|
| Enterprise | 99.95% | 15 min | 10% per 0.1% below |
| Pro | 99.9% | 1 hour | 5% per 0.1% below |
| Free | Best effort | Community | None |
```

---

## "Use this when..."

| Scenario | Document |
|----------|----------|
| New service entering production | Runbook + SLO |
| Alert fires with no documentation | Runbook |
| Production incident occurs | Incident Response Playbook |
| After incident is resolved | RCA (within 48 hours) |
| Setting service expectations with stakeholders | SLA |
| Defining monitoring thresholds | SLO |
