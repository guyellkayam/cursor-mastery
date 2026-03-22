# Security Workflows for Cursor

> **TL;DR**: Copy-paste security workflows — scanning pipelines, policy generation, compliance evidence, and threat modeling.

## Workflow 1: Security Scan Pipeline Setup

```
"Set up a comprehensive security scanning pipeline:

1. Pre-commit: Gitleaks for secret scanning
2. CI - SAST: Semgrep with OWASP Top 10 rules
3. CI - Container: Trivy scan on built images
4. CI - IaC: tfsec/checkov on Terraform files
5. CI - Dependencies: npm audit / pip-audit
6. Runtime: Falco rules for anomaly detection
7. Policy: Kyverno admission policies

Create GitHub Actions workflow for steps 2-5.
Create Helm values for Falco and Kyverno (steps 6-7).
Reference: @file:.github/workflows/ for existing patterns"
```

## Workflow 2: Kyverno Policy Generation

```
"Generate Kyverno ClusterPolicies for production security:

1. require-labels: All pods must have app, team, environment labels
2. require-resource-limits: All containers need CPU and memory limits
3. deny-privileged: Block privileged containers and hostPath mounts
4. require-nonroot: Enforce runAsNonRoot and readOnlyRootFilesystem
5. restrict-registries: Only allow images from approved registries
6. require-probes: All deployments must have readiness and liveness probes

For each policy:
- Write as Kyverno ClusterPolicy YAML
- Set validationFailureAction: Enforce
- Add audit annotation
- Test with a sample pod spec"
```

## Workflow 3: Threat Modeling

```
"Perform threat modeling for [service]:

Using STRIDE methodology:
1. Map the system: components, data flows, trust boundaries
2. For each boundary crossing, assess:
   - Spoofing: Can identity be faked?
   - Tampering: Can data be modified?
   - Repudiation: Can actions be denied?
   - Information Disclosure: Can data leak?
   - Denial of Service: Can service be disrupted?
   - Elevation of Privilege: Can access be escalated?
3. Rate each threat: High/Medium/Low impact × likelihood
4. Recommend mitigations for High and Medium threats
5. Generate threat model document

Output format from @file:resources/document-creation/security-and-compliance.md"
```

## Workflow 4: Compliance Evidence Collection

```
"Collect SOC 2 compliance evidence:

1. Access Control:
   - Screenshot SSO configuration
   - MFA enforcement proof
   - Last access review date

2. Change Management:
   - Branch protection rules (GitHub API)
   - PR review requirements
   - CI/CD pipeline evidence

3. Monitoring:
   - Alert configuration
   - Log retention policy
   - Incident response plan link

4. Encryption:
   - TLS certificate verification
   - At-rest encryption config

Format as SOC 2 evidence template from @file:resources/document-creation/security-and-compliance.md"
```
