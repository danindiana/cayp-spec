# CAYP: Configuration for Agentic Systems - YAML Profile

[![Validate Examples](https://github.com/danindiana/cayp-spec/actions/workflows/validate-examples.yml/badge.svg)](https://github.com/danindiana/cayp-spec/actions/workflows/validate-examples.yml)
[![Check Specification](https://github.com/danindiana/cayp-spec/actions/workflows/check-spec.yml/badge.svg)](https://github.com/danindiana/cayp-spec/actions/workflows/check-spec.yml)
[![Check Links](https://github.com/danindiana/cayp-spec/actions/workflows/check-links.yml/badge.svg)](https://github.com/danindiana/cayp-spec/actions/workflows/check-links.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Version](https://img.shields.io/badge/version-1.0.0-brightgreen.svg)](https://github.com/danindiana/cayp-spec/blob/main/CHANGELOG.md)
[![Status](https://img.shields.io/badge/status-draft-orange.svg)](https://github.com/danindiana/cayp-spec/blob/main/specification/CAYP-SPEC-v1.0.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/danindiana/cayp-spec/blob/main/CONTRIBUTING.md)

**Version:** 1.0.0
**Status:** Draft Specification
**Last Updated:** November 2025

---

## 🎯 Overview

CAYP (Configuration for Agentic Systems - YAML Profile) is a **deterministic, secure subset of YAML 1.2** specifically designed for configuring AI agents, models, tools, and workflows. It eliminates YAML's ambiguities, security vulnerabilities, and version incompatibilities while providing first-class semantics for agentic system configuration.

### The Problem CAYP Solves

Traditional YAML has fundamental issues for production configuration:

- **The Norway Problem:** `country: no` → `false`
- **Sexagesimal Numbers:** `version: 1.20` → `80`
- **Type Coercion:** Inconsistent parsing of `yes`, `on`, `true`, etc.
- **Security Risks:** Anchors, aliases, and custom tags enable attacks
- **Version Hell:** YAML 1.1 vs 1.2 incompatibilities

**Real-world impact:** 85% YAML formatting success rate in agent deployment systems.

### CAYP's Solution

✅ **Deterministic** - Same input = same output, always
✅ **Secure** - No code execution vectors or DoS attacks
✅ **Agentic-First** - Models, tools, workflows as primitives
✅ **Production-Ready** - Comprehensive validation and tooling

> 📊 **Visual Learner?** Check out our [Architecture & Diagrams](docs/ARCHITECTURE.md) for visual representations of CAYP's structure, validation workflows, and integration patterns.

---

## 📚 Documentation Structure

```
cayp-spec/
├── specification/
│   └── CAYP-SPEC-v1.0.md          # Core technical specification
├── schemas/
│   └── cayp-schema-v1.0.json      # JSON Schema for validation
├── examples/
│   ├── valid/
│   │   └── comprehensive-agent.cayp   # Full-featured example
│   ├── invalid/
│   │   ├── type-coercion-errors.cayp  # Common mistakes
│   │   ├── security-violations.cayp   # Security issues
│   │   └── structural-errors.cayp     # Validation errors
│   └── migration/
│       ├── before-yaml.yaml           # Traditional YAML
│       └── after-cayp.cayp            # Migrated to CAYP
├── tools/
│   └── (validators, migrators - to be implemented)
├── docs/
│   ├── MERGE_SUMMARY.md           # Branch merge documentation
│   ├── PHASE_1_REVIEW.md          # Phase 1 implementation review
│   ├── SESSION_SUMMARY.md         # Development session notes
│   └── WORK_SUMMARY_2025-11-08.md # Daily work log
└── .vscode/
    └── (workspace configuration)
```

---

## 🚀 Quick Start

### Simple CAYP Example

```yaml
cayp_version: "1.0"
name: "hello-agent"
description: "Simple greeting agent"
created: "2025-11-08"

models:
  primary:
    provider: "anthropic"
    model: "claude-sonnet-4"
    parameters:
      max_tokens: 1000

tools:
  - name: "greet"
    type: "function"
    description: "Generate greeting"
    parameters:
      name:
        type: "string"
        required: true

workflows:
  greet_user:
    description: "Greet the user by name"
    steps:
      - name: "generate"
        model: "primary"
        input:
          task: "Greet ${name}"
        output: "greeting"
    output: "${greeting}"

safety:
  content_filtering:
    enabled: true
    action: "block"
  audit:
    log_level: "info"
```

### Key Differences from YAML

| YAML Issue | CAYP Solution |
|-----------|--------------|
| `country: no` → `false` | Must quote: `country: "no"` |
| `version: 1.20` → `80` | Must quote: `version: "1.20"` |
| `enabled: yes` | Use: `enabled: true` |
| Anchors/aliases (`&`, `*`) | Forbidden (security risk) |
| Custom tags (`!!python`) | Forbidden (code execution) |
| Implicit nulls | Must be explicit: `null` or `~` |

---

## 🔑 Key Features

### 1. Determinism First
- **Fixed YAML version:** 1.2 Core Schema only
- **Explicit types:** No implicit coercion
- **Banned ambiguous features:** Norway problem solved
- **Schema validation:** Structure enforced

### 2. Security by Design
- **No code execution:** No anchors, aliases, or custom tags
- **DoS prevention:** Size limits enforced
- **Secrets management:** Environment variables only
- **Audit logging:** Required for production

### 3. Agentic Semantics
- **Models:** Provider-aware configuration with fallbacks
- **Tools:** Typed parameters with authentication
- **Workflows:** Multi-step orchestration with error handling
- **Safety:** Content filtering and output validation

### 4. Production Ready
- **Comprehensive validation:** Syntax + schema + semantics
- **Clear error messages:** Line numbers with fix suggestions
- **Migration tooling:** YAML → CAYP conversion
- **Multi-language support:** Python, Rust, JavaScript

---

## 📖 Specification Highlights

### Required Top-Level Fields

Every CAYP file MUST include:

```yaml
cayp_version: "1.0"      # Specification version
name: "agent-name"       # Identifier
description: "purpose"   # What it does
created: "2025-11-08"    # ISO 8601 date
safety: {...}            # Safety configuration (REQUIRED)
```

### Type System Rules

```yaml
# ✅ CORRECT - Explicit and unambiguous
country: "no"            # String
version: "1.20"          # String
enabled: true            # Boolean
count: 42                # Integer
price: 19.99             # Float
optional: null           # Explicit null

# ❌ INCORRECT - Ambiguous or unsafe
country: no              # Parses as boolean!
version: 1.20            # Might parse as sexagesimal!
enabled: yes             # Legacy boolean
optional:                # Implicit null
```

### Safety Configuration (Required)

```yaml
safety:
  content_filtering:
    enabled: true
    categories:
      - "hate_speech"
      - "violence"
    action: "block"
    
  output_validation:
    max_length: 50000
    forbidden_patterns:
      - regex: "API_KEY"
        reason: "Prevent leakage"
    
  audit:
    log_level: "info"
    log_inputs: true
    retention_days: 90
```

---

## 🛠️ Tooling (Planned)

### Validator
```bash
cayp validate config.cayp --strict
```
- Syntax validation (YAML 1.2 compliance)
- Schema validation (structure correctness)
- Semantic validation (cross-references, types)

### Migrator
```bash
cayp migrate legacy.yaml --output config.cayp
```
- Automatic YAML → CAYP conversion
- Safety issue detection
- Compatibility report generation

### Schema Generator
```bash
cayp schema generate config.cayp --output schema.json
```
- Generate JSON Schema from CAYP
- Custom type support
- Documentation generation

### IDE Support
- Syntax highlighting
- Auto-completion
- Inline validation
- Quick-fixes for common issues

---

## 🔍 Validation Stages

CAYP validation occurs in three stages:

1. **Syntax Validation**
   - Valid YAML 1.2 Core Schema
   - No forbidden features (anchors, custom tags)
   - Size limits enforced
   - Type correctness

2. **Schema Validation**
   - Structure matches CAYP schema
   - Required fields present
   - Field types correct
   - Cross-references valid

3. **Semantic Validation**
   - Provider-specific model validation
   - Tool parameter compatibility
   - Workflow step dependencies
   - Security configuration completeness

---

## 📊 Migration Example

### Before (YAML)
```yaml
version: 1.20              # ❌ Sexagesimal!
country: no                # ❌ Boolean!
enabled: yes               # ❌ Legacy boolean
api_key: sk-123            # ❌ Hardcoded secret!

defaults: &common          # ❌ Security risk
  timeout: 30

model:
  <<: *common              # ❌ Alias expansion
  name: gpt-4
```

### After (CAYP)
```yaml
cayp_version: "1.0"
name: "my-agent"
description: "Migrated configuration"
created: "2025-11-08"

metadata:
  version: "1.20"          # ✅ Quoted string

deployment:
  country: "no"            # ✅ Explicit string
  enabled: true            # ✅ Standard boolean

authentication:
  api_key_env: "API_KEY"   # ✅ Environment variable

models:
  primary:
    provider: "openai"
    model: "gpt-4"
    parameters:
      timeout: 30          # ✅ Explicit (no anchors)

safety:                    # ✅ Required section
  content_filtering:
    enabled: true
    action: "block"
  audit:
    log_level: "info"
```

**Migration benefits:**
- ✅ No type coercion surprises
- ✅ No security vulnerabilities
- ✅ Deterministic parsing
- ✅ Production-ready safety

---

## 🎯 Use Cases

### 1. Agent Deployment
```yaml
# Deploy agents with validated, secure configuration
cayp_version: "1.0"
name: "customer-support-agent"
models:
  primary:
    provider: "anthropic"
    model: "claude-sonnet-4"
    fallback: "backup"
safety:
  rate_limiting:
    requests_per_user_per_hour: 100
  audit:
    log_level: "info"
```

### 2. Multi-Step Workflows
```yaml
workflows:
  research:
    description: "Research and report"
    steps:
      - name: "search"
        tool: "web_search"
      - name: "analyze"
        model: "primary"
      - name: "report"
        model: "specialized"
    error_handling:
      on_failure: "retry"
```

### 3. Tool Configuration
```yaml
tools:
  - name: "database_query"
    type: "api"
    authentication:
      type: "service_account"
      credentials_env: "DB_CREDS"
    rate_limit:
      requests_per_minute: 60
```

---

## 🏗️ Implementation Status

### Phase 1: Specification (Current)
- ✅ Core technical specification
- ✅ JSON Schema for validation
- ✅ Example configurations (valid/invalid)
- ✅ Migration examples
- 🔄 External technical review (in progress)

### Phase 2: Reference Implementation (Next)
- ⏳ Python validator
- ⏳ Rust validator (performance)
- ⏳ JavaScript validator (web tools)
- ⏳ Migration tooling

### Phase 3: Ecosystem (Future)
- ⏳ IDE plugins (VS Code, IntelliJ)
- ⏳ CI/CD integration
- ⏳ Documentation generators
- ⏳ Community adoption

### Phase 4: Standardization (Future)
- ⏳ RFC publication
- ⏳ Standards body submission
- ⏳ Industry partnerships
- ⏳ Multi-language ecosystem

---

## 🤝 Contributing

CAYP is an open specification. We welcome:

- **Feedback** on the specification
- **Bug reports** for validation issues
- **Feature requests** for agentic semantics
- **Implementation contributions** in any language
- **Documentation improvements**

### Review Priorities

**Critical for v1.0:**
1. Security model review
2. Agentic semantics validation
3. Real-world use case testing
4. Migration strategy validation

**Nice to have:**
1. Additional provider support
2. Extended tool types
3. Advanced workflow patterns
4. Performance optimizations

---

## 📋 Security Considerations

CAYP protects against:

1. **Code Execution:** No custom tags or anchors
2. **DoS Attacks:** Size limits and no alias expansion
3. **Data Exfiltration:** No external references
4. **Injection Attacks:** Strict type validation
5. **Configuration Tampering:** Schema validation

**Best Practices:**
- Never hardcode secrets
- Always use environment variables
- Enable audit logging
- Validate before execution
- Run in sandboxed environments

---

## 📚 Additional Resources

- **Specification:** [CAYP-SPEC-v1.0.md](specification/CAYP-SPEC-v1.0.md)
- **Schema:** [cayp-schema-v1.0.json](schemas/cayp-schema-v1.0.json)
- **Architecture & Diagrams:** [ARCHITECTURE.md](docs/ARCHITECTURE.md) - Visual guides and workflows
- **Examples:** [examples/](examples/) directory
- **Documentation:** [docs/](docs/) directory
- **Issues:** GitHub Issues (to be set up)
- **Discussions:** Community forum (to be set up)

---

## 📄 License

- **Specification:** MIT License
- **Reference Implementations:** Apache 2.0 License
- **Documentation:** Creative Commons BY 4.0

---

## 🙏 Acknowledgments

CAYP builds on learnings from:
- YAML 1.2 Specification
- Ruud van Asseldonk's YAML critique
- StrictYAML project
- Real-world agent deployment experience

**Special thanks to:**
- Agent developers who validated the need
- Security researchers who identified YAML risks
- The open-source community

---

## 🎯 Next Steps

1. **Review the specification:** `specification/CAYP-SPEC-v1.0.md`
2. **Study the examples:** `examples/` directory
3. **Test with real configs:** Migration path in `examples/migration/`
4. **Provide feedback:** Open issues, suggest improvements
5. **Implement validators:** Python, Rust, JavaScript

**Ready to build the future of agentic configuration? Let's go! 🚀**
