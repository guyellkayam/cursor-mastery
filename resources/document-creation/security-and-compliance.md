# Security & Compliance Documentation

> **TL;DR**: Templates for security audit reports, compliance documentation (SOC2, GDPR, ISO27001), CIS benchmark evidence, and penetration test reports.

## Security Audit Report

### Template
```markdown
# Security Audit Report

**Date**: [YYYY-MM-DD]
**Scope**: [systems/services audited]
**Auditor**: [name/team]
**Classification**: Confidential

## Executive Summary
[2-3 sentences: overall security posture, critical findings count, recommendation]

## Scope
- **Systems**: [list of systems audited]
- **Period**: [date range]
- **Methodology**: [OWASP, NIST, custom]

## Findings Summary
| ID | Finding | Severity | Status | CVSS |
|----|---------|----------|--------|------|
| F-1 | [finding] | Critical | Open | 9.8 |
| F-2 | [finding] | High | Remediated | 7.5 |
| F-3 | [finding] | Medium | Open | 5.3 |

## Detailed Findings

### F-1: [Finding Title]
- **Severity**: Critical
- **CVSS Score**: 9.8
- **Affected Systems**: [list]
- **Description**: [detailed description]
- **Evidence**: [how it was found/reproduced]
- **Impact**: [what could happen if exploited]
- **Recommendation**: [how to fix]
- **Timeline**: Remediate within [X] days

## Risk Matrix
| | Low Likelihood | Medium | High |
|---|---|---|---|
| **High Impact** | Medium Risk | High Risk | Critical Risk |
| **Medium Impact** | Low Risk | Medium Risk | High Risk |
| **Low Impact** | Info | Low Risk | Medium Risk |

## Recommendations Summary
1. [Priority 1 action]
2. [Priority 2 action]
3. [Priority 3 action]

## Next Steps
- [ ] Remediate critical findings within 7 days
- [ ] Remediate high findings within 30 days
- [ ] Schedule follow-up audit in [timeframe]
```

---

## SOC 2 Evidence Template

### Template
```markdown
# SOC 2 Control Evidence

## Control: [CC-X.X] [Control Name]

### Trust Service Criteria
**Category**: Security | Availability | Processing Integrity | Confidentiality | Privacy

### Control Description
[What the control does and why it's needed]

### Implementation Evidence

#### Access Control (CC6.1)
- **SSO Provider**: [provider]
- **MFA Enforcement**: [screenshot/config evidence]
- **Access Review Cadence**: Quarterly
- **Evidence**: [link to access review document]

#### Change Management (CC8.1)
- **PR Review Required**: Yes (branch protection rules)
- **CI/CD Pipeline**: [link to pipeline config]
- **Approval Process**: [description]
- **Evidence**: [link to sample PR with reviews]

#### Monitoring & Logging (CC7.2)
- **Log Aggregation**: [tool]
- **Retention Period**: [X days]
- **Alert Configuration**: [link to alert rules]
- **Evidence**: [link to dashboard screenshot]

### Gaps & Remediation
| Gap | Remediation Plan | Due Date | Owner |
|-----|-----------------|----------|-------|
| [gap] | [plan] | [date] | [name] |
```

---

## GDPR Compliance

### Data Processing Record Template
```markdown
# Record of Processing Activities (GDPR Article 30)

## Processing Activity: [Name]

| Field | Details |
|-------|---------|
| **Data Controller** | [organization name] |
| **Purpose** | [why data is processed] |
| **Legal Basis** | Consent / Contract / Legitimate Interest / Legal Obligation |
| **Data Categories** | [personal data types: name, email, IP, etc.] |
| **Data Subjects** | [who: customers, employees, etc.] |
| **Recipients** | [who receives: internal teams, processors] |
| **Third Countries** | [transfers outside EU/EEA] |
| **Retention Period** | [how long data is kept] |
| **Security Measures** | Encryption, access control, pseudonymization |
| **DPIA Required** | Yes / No |

## Data Subject Rights Implementation
- [ ] Right of Access (Article 15)
- [ ] Right to Rectification (Article 16)
- [ ] Right to Erasure (Article 17)
- [ ] Right to Restrict Processing (Article 18)
- [ ] Right to Data Portability (Article 20)
- [ ] Right to Object (Article 21)
```

---

## CIS Benchmark Evidence

### Template
```markdown
# CIS Benchmark Compliance: [Platform]

**Benchmark Version**: [e.g., CIS AWS Foundations 3.0]
**Assessment Date**: [date]
**Scope**: [account/subscription/cluster]

## Compliance Summary
- **Total Controls**: [N]
- **Compliant**: [X] ([X/N]%)
- **Non-Compliant**: [Y]
- **Not Applicable**: [Z]

## Non-Compliant Controls

### [Control ID]: [Control Title]
- **Level**: 1 (essential) / 2 (defense in depth)
- **Current State**: [what exists now]
- **Required State**: [what CIS requires]
- **Remediation**: [how to fix]
- **Priority**: [High/Medium/Low]
- **Owner**: [name]
- **Target Date**: [date]

## Automated Scanning
- **Tool**: [AWS Config, Azure Policy, kube-bench, etc.]
- **Frequency**: [daily, weekly]
- **Dashboard**: [link]
```

---

## Penetration Test Report Summary

### Template
```markdown
# Penetration Test Report

**Test Date**: [date range]
**Tester**: [firm/individual]
**Scope**: [URLs, IPs, apps tested]
**Methodology**: OWASP WSTG / PTES / Custom

## Executive Summary
[3-4 sentences: scope, approach, key findings, overall risk level]

## Findings
| # | Title | Severity | CVSS | Status |
|---|-------|----------|------|--------|
| 1 | [finding] | Critical | 9.1 | Open |
| 2 | [finding] | High | 7.8 | Open |

## Remediation Priority
1. **Immediate** (0-7 days): [critical findings]
2. **Short-term** (30 days): [high findings]
3. **Medium-term** (90 days): [medium findings]

## Retesting
Recommended retest after critical/high findings are remediated.
```

---

## "Use this when..."

| Scenario | Document |
|----------|----------|
| Annual security review | Security Audit Report |
| SOC 2 Type II audit prep | SOC 2 Evidence Template |
| Processing personal data in EU | GDPR Data Processing Record |
| Cloud infrastructure compliance | CIS Benchmark Evidence |
| Third-party security assessment | Pen Test Report |
