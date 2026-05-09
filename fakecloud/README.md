# VentureMind on fakecloud — Local AWS Emulation

> 33 services · 2,422 operations · Zero cloud cost in dev/test

---

## What Is fakecloud?

**fakecloud** (faiscadev, AGPL-3.0) is a free, open-source local AWS emulator. Runs as a single 19MB Rust binary on `http://localhost:4566` — no account, no token, no paid tier. Replaces LocalStack Community at 10× faster startup, 1/15th the memory, and covers 22 LocalStack paid-tier services for free.

### Key Stats

| Metric | Value |
|--------|-------|
| Services | 33 |
| Operations | 2,422 |
| Startup time | ~500ms |
| Idle memory | ~10 MiB |
| Binary size | ~19 MB |
| License | AGPL-3.0 |
| Stars | 283 |

### Services That Were Paid in LocalStack (Free in fakecloud)

Cognito User Pools · SES v2 · SES inbound · RDS · ElastiCache · API Gateway v1+v2 · Bedrock · ECR · ECS · ELB v2 · CloudFront · Lambda · DynamoDB · S3 · SQS · SNS · EventBridge · IAM · STS · KMS · Secrets Manager · CloudFormation · SSM · CloudWatch Logs · Kinesis · Step Functions · ACM

---

## VentureMind × fakecloud

VentureMind's Engineering Swarm uses fakecloud to test every AWS integration before deploying to real infrastructure. No cloud bills from development. No account required. Full conformance testing locally.

### Architecture

```
VentureMind Engineering Swarm
├── fakecloud-test-harness (SOUL.md)
│   └── Coordinates fakecloud lifecycle + test orchestration
├── venturemind-aws-test-harness (SKILL.md)
│   └── Runs integration test suites against fakecloud
└── infra-manifests/
    └── fakecloud-stack.md — startup config + SDK wiring + test data seeding
```

### What Gets Tested

| Component | Service | What's Tested |
|-----------|---------|---------------|
| Auth | Cognito | User pools, MFA, OIDC/SAML, all 12 triggers |
| Email | SES | Send, templates, DKIM, receipt rules |
| Database | RDS | PostgreSQL/MySQL/MariaDB via real Docker |
| Cache | ElastiCache | Redis/Valkey real containers |
| Compute | Lambda | 23 runtimes in real Docker, ESM |
| Storage | S3 | Multipart, versioning, lifecycle, SSE-KMS |
| Queues | SQS + SNS | FIFO, DLQs, fan-out, filter policies |
| Events | EventBridge | Pattern matching, schedules, archives |
| AI Inference | Bedrock | Foundation models, guardrails, runtime |
| API Gateway | API GW v1+v2 | REST/HTTP APIs, Lambda proxy, authorizers |
| Containers | ECS + ECR | Full API, real task execution |
| DNS + CDN | CloudFront + Route53 | Distributions, OAC, invalidations |

### CI/CD Integration

```yaml
# GitHub Actions — venturemind-fakecloud-action
- name: Start fakecloud
  run: |
    curl -fsSL https://raw.githubusercontent.com/faiscadev/fakecloud/main/install.sh | bash
    fakecloud &
    sleep 3

- name: Run VentureMind integration tests
  env:
    AWS_ACCESS_KEY_ID: test
    AWS_SECRET_ACCESS_KEY: test
    AWS_DEFAULT_REGION: us-east-1
  run: |
    # All tests point to http://localhost:4566
    # No real AWS credentials needed in CI
    pytest tests/ --tb=short
```

### LocalStack vs fakecloud Comparison

| Feature | fakecloud | LocalStack Community |
|---------|-----------|---------------------|
| Auth required | No | Yes (account + token) |
| Commercial use | Free | Paid |
| Startup | 500ms | ~3s |
| Memory | 10 MiB | ~150 MiB |
| Binary size | 19 MB | ~1 GB Docker |
| Cognito | 122 ops (free) | Paid |
| SES | Full send + inbound (free) | Paid |
| RDS | 163 ops (free) | Paid |
| Bedrock | 111 ops (free) | Not available |
| CloudFront | 147 ops (free) | Paid |
| ECS | 60 ops (free) | Paid |

---

## Quick Start

```bash
# Install fakecloud
curl -fsSL https://raw.githubusercontent.com/faiscadev/fakecloud/main/install.sh | bash

# Run fakecloud (background)
fakecloud &

# Verify it's up
curl http://localhost:4566/_localstack/health | jq

# Run VentureMind test suite
cd venturemind
pytest tests/integration/ --aws-endpoint-url=http://localhost:4566
```

---

*Built on [faiscadev/fakecloud](https://github.com/faiscadev/fakecloud) · AGPL-3.0 · Rust*