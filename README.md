# Sentinel Secrets

Context-aware GitHub Action for detecting leaked secrets and credential risks with low false positives.

## Why this exists
Sentinel Secrets keeps pull requests free of high-impact secrets without slowing down CI or drowning reviewers in noise.

### Threat Model
Sentinel Secrets is designed to detect accidental exposure of high-risk secrets
introduced during development workflows, primarily through pull requests.

It focuses on secrets that:
- Provide direct access to infrastructure, cloud resources, or third-party services
- Are likely to be exploitable if leaked
- Are introduced into version control by mistake

## Features

### Detects
- Cloud provider credentials (AWS, GCP, Azure)
- GitHub tokens and personal access tokens
- CI/CD secrets in workflow files
- High-entropy credentials used in authentication contexts
- Secrets introduced in pull requests or recent commit history

### Explicitly out of scope
- Test or mock credentials
- Low-risk API keys without write or admin privileges
- Hashed passwords or encrypted blobs
- Secrets already stored in approved secret managers
- Runtime secrets injected through environment variables

## Security Philosophy
Sentinel Secrets prioritizes signal over noise.

A secret is reported only when there is high confidence that:
1. The value is sensitive
2. The value is used in a security-relevant context
3. Exposure would have real-world impact

When confidence is low, Sentinel Secrets prefers silence.
