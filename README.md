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

## How it works

Sentinel Secrets uses a layered detection pipeline to maximize confidence while minimizing noise.

### 1. Candidate Extraction
Only newly introduced or modified lines in pull requests are analyzed.
This reduces scan time and avoids legacy noise.

### 2. Secret Likelihood Analysis
Candidate values are evaluated using:
- Known credential patterns
- Entropy scoring
- Length and character distribution checks

Only high-likelihood candidates proceed further.

### 3. Context Validation
Candidates are analyzed in their usage context using AST inspection.
Secrets are reported only when they are used in security-relevant operations
such as authentication, authorization, or infrastructure access.

### Security-relevant contexts include:
- Assignment to environment variables
- Use in SDK or client authentication
- Inclusion in HTTP authorization headers
- Usage in infrastructure or CI configuration

### Confidence scoring
Each finding is assigned a confidence score based on:
- Secret type certainty
- Entropy and structure
- Usage context
- Exposure surface

Findings below the confidence threshold are not reported.
