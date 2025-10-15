# Context Files - The Header Files of Intent

## The Problem

AI doesn't know your:
- Project structure
- Dependencies available
- Framework choices
- Constraints
- Environment

You need to tell it.

## The Solution: Context Section

Like C header files, but for intent compilation.

## Basic Example

```markdown
# My Feature

## Context
```yaml
framework: dbbasic-web
database: postgresql
python_version: 3.10+
dependencies:
  - requests
  - pydantic
```

## Feature Description
[Your intent here...]
```

## What Goes in Context?

### 1. Framework/Environment
```yaml
framework: dbbasic-web
database: dbbasic-tsv
storage_path: ./data
```

### 2. Dependencies
```yaml
dependencies:
  - requests
  - pandas
  - bcrypt
  - jwt
```

### 3. Constraints
```yaml
constraints:
  max_memory: 512MB
  target_platform: linux
  python_version: 3.10+
  no_external_services: true
```

### 4. Project Structure
```yaml
project_structure:
  base_path: ./src
  config_path: ./config
  output_path: ./api
```

### 5. Existing Code to Use
```yaml
existing_code:
  - ./lib/auth.py (has authenticate() function)
  - ./lib/db.py (has query() function)
  - ./models/user.py (has User class)
```

### 6. API/Integration Requirements
```yaml
integrations:
  stripe_api: true
  sendgrid_email: true
  redis_cache: localhost:6379
```

## Full Example

```markdown
# Payment Processing System

## Context
```yaml
# Framework
framework: dbbasic-web
database: postgresql
cache: redis

# Environment
python_version: 3.10+
deployment: docker

# Dependencies
dependencies:
  - stripe
  - pydantic
  - sqlalchemy
  - redis

# Constraints
constraints:
  max_response_time: 2s
  idempotent: true
  audit_logging: required

# Existing Code
existing:
  - ./lib/auth.py (JWT authentication)
  - ./models/user.py (User model)
  - ./lib/email.py (send_email function)

# Integrations
integrations:
  stripe_api_key: env.STRIPE_KEY
  webhook_secret: env.STRIPE_WEBHOOK_SECRET

# Project Structure
paths:
  output: ./api/payments
  logs: ./logs
  config: ./config
```

## Feature Description

Create a payment processing system.

[Rest of intent...]
```

## Why This Works

**Without context:**
```
AI generates:
- Generic Python code
- Assumes Flask/Django
- Uses libraries you don't have
- Doesn't fit your project
```

**With context:**
```
AI generates:
- DBBasic-compatible code
- Uses your existing auth
- Integrates with your database
- Follows your patterns
```

## Context Inheritance

You can have project-level context:

**`project.context.yaml`:**
```yaml
framework: dbbasic-web
database: dbbasic-tsv
python_version: 3.10+
dependencies:
  - bcrypt
  - jwt
```

**Then in intent files:**
```markdown
# User API

## Context
inherit: ./project.context.yaml
additional_dependencies:
  - pydantic
```

## Context Templates

Common contexts for quick start:

**DBBasic Web API:**
```yaml
framework: dbbasic-web
database: dbbasic-tsv
dependencies: [bcrypt, jwt]
```

**Data Processing:**
```yaml
dependencies: [pandas, numpy]
constraints:
  memory_efficient: true
```

**Microservice:**
```yaml
framework: fastapi
database: postgresql
cache: redis
deployment: docker
```

## Validation

The compiler can validate context:
```bash
$ dbbasic-compile --validate-context myapp.intent.md
✓ Framework: dbbasic-web (available)
✓ Dependencies: all installed
✗ Database: mysql (not configured)
```

## Best Practices

### 1. Start Minimal
```yaml
# Just what you need
framework: dbbasic-web
database: dbbasic-tsv
```

### 2. Be Specific
```yaml
# Good
python_version: 3.10+
# Bad
python_version: latest
```

### 3. Document Existing Code
```yaml
existing_code:
  - ./lib/auth.py
    # Has: authenticate(token) -> User
    # Has: generate_token(user) -> str
```

### 4. Include Constraints
```yaml
constraints:
  no_external_api_calls: true  # Offline mode
  max_dependencies: 5          # Keep it light
  standard_library_preferred: true
```

## Future: Smart Defaults

The compiler could learn your project:

```bash
$ dbbasic-compile --init
Analyzing project...
Found: requirements.txt, dbbasic setup
Generated: project.context.yaml

$ cat project.context.yaml
# Auto-detected project context
framework: dbbasic-web
database: dbbasic-tsv
dependencies: [bcrypt, jwt, pydantic]
python_version: 3.10
```

## The Mental Model

**C Programming:**
```c
#include <stdio.h>      // Tell compiler what's available
#include "mylib.h"      // Tell compiler about your code

int main() {            // Your code
    printf("Hello");
}
```

**Intent Programming:**
```markdown
## Context              <!-- Tell compiler what's available -->
framework: dbbasic-web
existing: ./lib/auth.py

## Intent               <!-- Your requirements -->
Create login system
```

Same concept. Different layer.

Context tells the compiler: "Here's what you have to work with."
Intent tells the compiler: "Here's what I want built."

Together, you get code that actually works in your environment.
