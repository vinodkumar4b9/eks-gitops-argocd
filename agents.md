# AGENTS.md

> This file provides instructions and context for AI coding agents (Amazon Q Developer, GitHub Copilot, Cursor, Kiro, Claude Code, etc.) working in this repository.

---

## Project Overview

- **Project Name:** [sample project -4]
- **Description:** [Brief description of what this application does]
- **Tech Stack:** [e.g., Python 3.12, FastAPI, AWS CDK, DynamoDB, Lambda]
- **Architecture:** [e.g., Microservices / Monolith / Serverless]
- **Cloud Provider:** AWS
- **Repository Type:** [e.g., Backend API / Infrastructure / Full-stack / Library]

---

## Directory Structure

```
├── src/                    # Application source code
│   ├── handlers/           # Lambda handlers / API route handlers
│   ├── models/             # Data models / Pydantic schemas
│   ├── services/           # Business logic layer
│   ├── utils/              # Shared utilities and helpers
│   └── config/             # Configuration and environment management
├── infra/                  # Infrastructure as Code (CDK / Terraform)
├── tests/
│   ├── unit/               # Unit tests
│   ├── integration/        # Integration tests
│   └── e2e/                # End-to-end tests
├── scripts/                # Build, deploy, and utility scripts
├── docs/                   # Documentation
└── .github/                # CI/CD workflows
```

---

## Coding Standards

### General Rules
- Follow **PEP 8** for Python (or specify your language's style guide)
- Use **type hints** on all function signatures
- Maximum line length: **120 characters**
- Use **meaningful variable and function names** — no single-letter variables except in loops
- All public functions must have **docstrings** (Google-style)

### Naming Conventions
| Element | Convention | Example |
|---------|-----------|---------|
| Files / Modules | snake_case | `user_service.py` |
| Classes | PascalCase | `UserService` |
| Functions / Methods | snake_case | `get_user_by_id()` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Environment Variables | UPPER_SNAKE_CASE | `DATABASE_URL` |

### Error Handling
- Use custom exception classes defined in `src/utils/exceptions.py`
- Never catch generic `Exception` without re-raising or logging
- All external API calls must have retry logic and timeout handling
- Use structured logging (JSON format) via the project logger

### Security
- Never hardcode secrets, API keys, or credentials
- Use AWS Secrets Manager or environment variables for sensitive values
- Validate and sanitize all user inputs
- Follow least-privilege IAM principles in infrastructure code

---

## Testing Requirements

- **Unit test coverage:** Minimum 80%
- **Testing framework:** pytest
- **Mocking:** Use `moto` for AWS services, `unittest.mock` for other dependencies
- **Test file naming:** `test_<module_name>.py`
- **Run tests:** `pytest tests/ -v --cov=src --cov-report=term-missing`

### Test Structure
```python
def test_<function_name>_<scenario>_<expected_outcome>():
    # Arrange
    ...
    # Act
    ...
    # Assert
    ...
```

---

## Git & Branching

- **Main branch:** `main` (production-ready)
- **Development:** `dev` (integration branch)
- **Feature branches:** `feature/<ticket-id>-<short-description>`
- **Commit messages:** Follow [Conventional Commits](https://www.conventionalcommits.org/)
  - `feat:` new feature
  - `fix:` bug fix
  - `refactor:` code refactoring
  - `docs:` documentation changes
  - `test:` adding/updating tests
  - `chore:` maintenance tasks

---

## AWS-Specific Guidelines

- **Region:** ap-south-1 (Mumbai) unless otherwise specified
- **IaC Tool:** [AWS CDK (Python) / Terraform]
- **Deployment:** [CodePipeline / GitHub Actions]
- **Logging:** Use structured JSON logs; integrate with CloudWatch Logs
- **Monitoring:** CloudWatch Metrics + Alarms for all critical paths
- **Tagging:** All resources must include tags: `Project`, `Environment`, `Owner`, `CostCenter`

### Lambda Functions
- Maximum timeout: 30 seconds (unless processing-heavy)
- Use Powertools for Lambda (logging, tracing, metrics)
- Bundle dependencies with Lambda layers for shared code

### DynamoDB
- Use single-table design where appropriate
- Always define TTL for temporary/session data
- Use `boto3` resource interface for simple CRUD, client interface for complex queries

---

## Dependencies & Package Management

- **Package manager:** [pip + requirements.txt / Poetry / pipenv]
- **Lock file:** Always commit the lock file
- **Adding dependencies:** Document why in the PR description
- **Vulnerable packages:** Run `pip-audit` or `safety check` before merging

---

## CI/CD Pipeline

Automated checks on every PR:
1. ✅ Linting (`ruff` / `flake8`)
2. ✅ Type checking (`mypy`)
3. ✅ Unit tests (`pytest`)
4. ✅ Security scan (`bandit` / `pip-audit`)
5. ✅ Infrastructure validation (`cdk synth` / `terraform plan`)

---

## Do NOT

- ❌ Commit directly to `main` or `dev`
- ❌ Use `print()` for logging — use the structured logger
- ❌ Create AWS resources without IaC
- ❌ Store state locally — always use DynamoDB / S3 / Parameter Store
- ❌ Skip error handling for external API calls
- ❌ Use `*` imports
- ❌ Leave TODO comments without a linked ticket

---

## Helpful Context for Agents

- When generating infrastructure code, always use the patterns in `infra/constructs/`
- Reuse existing utility functions in `src/utils/` before writing new ones
- Check `docs/adr/` for Architecture Decision Records before proposing major changes
- The project uses [dependency injection / factory pattern / repository pattern] — follow existing patterns
- API responses follow the standard envelope: `{"status": "success", "data": {...}, "metadata": {...}}`

---

## Contact

- **Team:** [Your Team Name]
- **Slack:** [#your-team-channel]
- **On-call:** [Rotation link or PagerDuty]
