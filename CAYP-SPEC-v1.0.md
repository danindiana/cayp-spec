# CAYP Specification v1.0
## Configuration for Agentic Systems - YAML Profile

**Status:** Draft  
**Version:** 1.0.0  
**Date:** November 2025  
**Authors:** CAYP Working Group

---

## Abstract

CAYP (Configuration for Agentic Systems - YAML Profile) is a deterministic, secure subset of YAML 1.2 specifically designed for configuring AI agents, models, tools, and workflows. It eliminates YAML's ambiguities, security vulnerabilities, and version incompatibilities while providing first-class semantics for agentic system configuration.

**Key Differentiators:**
- Deterministic parsing (no type coercion surprises)
- Security-first design (no code execution vectors)
- Agentic semantics (models, tools, safety as primitives)
- Production-grade validation (schema + semantic checks)

---

## 1. Motivation

### 1.1 The YAML Problem

YAML, while popular, has fundamental issues for production configuration:

1. **The Norway Problem:** `country: no` parses as `country: false`
2. **Sexagesimal Surprises:** `version: 1.20` becomes `version: 80` (1*60 + 20)
3. **Type Coercion Chaos:** Inconsistent parsing of `yes`, `on`, `off`, `true`, `1`, etc.
4. **Version Incompatibility:** YAML 1.1 vs 1.2 parse identically-looking documents differently
5. **Security Vectors:** Anchors, aliases, and custom tags enable code execution
6. **Indeterminate Behavior:** Same input → different outputs across parsers

**Real-World Impact:**
- 85% YAML formatting success rate in agent deployment systems
- Security vulnerabilities from alias expansion attacks
- Production outages from version/type coercion issues

### 1.2 Why Not JSON?

JSON solves determinism but lacks:
- Human readability for complex configs
- Comments for documentation
- Multi-line string support
- Type richness needed for agentic systems

### 1.3 CAYP's Solution

CAYP provides:
- ✅ YAML 1.2's readability and features
- ✅ JSON's determinism and safety
- ✅ Domain-specific semantics for AI agents
- ✅ Production-grade validation and tooling

---

## 2. Design Principles

### 2.1 Determinism First
**Principle:** Identical input must produce identical parsed output across all compliant parsers.

**Implementation:**
- Explicit types only (no implicit coercion)
- Fixed YAML version (1.2, no fallback)
- Banned ambiguous features
- Schema-validated structure

### 2.2 Security by Design
**Principle:** Configuration files cannot execute code or cause resource exhaustion.

**Implementation:**
- No anchors or aliases (DoS prevention)
- No custom tags (no code execution)
- No external references (no SSRF)
- Size limits enforced (no memory attacks)

### 2.3 Agentic Semantics
**Principle:** First-class support for AI agent configuration patterns.

**Implementation:**
- Native `models` type with provider-aware validation
- `tools` and `capabilities` as core concepts
- `workflows` and `orchestration` primitives
- `safety` and `constraints` as required fields

### 2.4 Production Ready
**Principle:** Built for real-world deployment from day one.

**Implementation:**
- Comprehensive validation (syntax + semantics)
- Clear error messages with fix suggestions
- Migration tooling from YAML
- Multi-language reference implementations

---

## 3. Technical Specification

### 3.1 Base Syntax

CAYP uses **YAML 1.2 Core Schema** with restrictions.

**Allowed:**
- Mappings (key-value pairs)
- Sequences (lists)
- Scalars (strings, numbers, booleans, null)
- Comments (# style only)
- Multi-line strings (literal | and folded >)
- Explicit tags for disambiguation (!!str, !!int, !!float, !!bool)

**Forbidden:**
- Anchors (&) and aliases (*)
- Custom tags (except !!str, !!int, !!float, !!bool, !!null)
- Merge keys (<<)
- Multiple documents in one file (---)
- Binary data (!!binary)
- YAML 1.1 compatibility mode

### 3.2 Type System

CAYP enforces explicit typing with strict rules:

#### 3.2.1 Strings
```yaml
# ✅ CORRECT - Quoted ambiguous values
country: "no"
status: "yes"
value: "on"
version: "1.20"

# ❌ INCORRECT - Unquoted ambiguous values
country: no        # Parses as boolean false
status: yes        # Parses as boolean true
version: 1.20      # Could parse as sexagesimal
```

**Rules:**
- Ambiguous strings MUST be quoted: `yes`, `no`, `on`, `off`, `true`, `false`
- Version numbers MUST be quoted: `"1.20"`, `"2.0"`
- Numeric-like strings MUST be quoted: `"123abc"`, `"0x10"`

#### 3.2.2 Numbers
```yaml
# ✅ CORRECT
count: 42
price: 19.99
scientific: 1.5e10

# ❌ INCORRECT
octal: 0o755      # No octal notation
hex: 0xFF         # No hex notation
sexagesimal: 1:20 # No sexagesimal (use 80 or "1:20")
```

**Rules:**
- Integers: Plain decimal only
- Floats: Decimal or scientific notation only
- No alternative number bases

#### 3.2.3 Booleans
```yaml
# ✅ CORRECT - Only true/false
enabled: true
disabled: false

# ❌ INCORRECT - Legacy values forbidden
enabled: yes      # Use true
enabled: on       # Use true
enabled: 1        # Use true
```

**Rules:**
- Only `true` and `false` are valid booleans
- Quote legacy values: `status: "yes"` if you need the string

#### 3.2.4 Null
```yaml
# ✅ CORRECT
optional_field: null
other_field: ~

# ❌ INCORRECT
optional_field:    # Implicit null forbidden
```

**Rules:**
- Explicit null only: `null` or `~`
- Empty values must be explicitly null

### 3.3 Structural Rules

#### 3.3.1 Root Structure
```yaml
# ✅ CORRECT - Single document
cayp_version: "1.0"
name: "agent-config"
# ... rest of config

# ❌ INCORRECT - Multiple documents
---
config1: value
---
config2: value
```

**Rules:**
- One document per file
- Must start with `cayp_version` field

#### 3.3.2 Key Naming
```yaml
# ✅ CORRECT - Clear, unambiguous keys
model_name: "gpt-4"
max_tokens: 1000
enable_logging: true

# ❌ INCORRECT - Ambiguous or dangerous keys
model: gpt-4           # Could conflict with reserved 'model' type
'yes': value           # Ambiguous key
"": value              # Empty key
```

**Rules:**
- Keys must be non-empty strings
- Keys should not be YAML boolean literals
- Keys should use snake_case for consistency

### 3.4 Size Limits

To prevent resource exhaustion:

```yaml
# Maximum sizes (MUST be enforced)
max_file_size: 10MB
max_nesting_depth: 20
max_sequence_length: 10000
max_string_length: 1MB
max_keys_per_mapping: 1000
```

**Rules:**
- Parsers MUST reject files exceeding limits
- Limits are for total file, not per-structure
- Errors MUST specify which limit was exceeded

---

## 4. Agentic Semantics

CAYP provides first-class support for AI agent configuration.

### 4.1 Required Top-Level Fields

Every CAYP file MUST include:

```yaml
cayp_version: "1.0"           # REQUIRED: CAYP spec version
name: "my-agent"              # REQUIRED: Human-readable identifier
description: "Agent purpose"  # REQUIRED: What this agent does
created: "2025-11-08"         # REQUIRED: ISO 8601 date
```

### 4.2 Model Configuration

Define AI models with provider-aware validation:

```yaml
models:
  primary:
    provider: "anthropic"     # REQUIRED: anthropic, openai, etc.
    model: "claude-sonnet-4"  # REQUIRED: Model identifier
    parameters:
      max_tokens: 4096
      temperature: 0.7
      top_p: 0.9
    fallback: "secondary"     # OPTIONAL: Fallback model name
    
  secondary:
    provider: "openai"
    model: "gpt-4"
    parameters:
      max_tokens: 4096
```

**Validation Rules:**
- `provider` must be from known list (extensible)
- `model` must be valid for provider
- `parameters` validated against provider schema
- `fallback` must reference another defined model

### 4.3 Tools and Capabilities

Define agent capabilities with type safety:

```yaml
tools:
  - name: "web_search"
    type: "function"          # function, api, system
    description: "Search the web for current information"
    parameters:
      query:
        type: "string"
        required: true
      max_results:
        type: "integer"
        default: 10
    authentication:
      type: "api_key"
      key_env: "SEARCH_API_KEY"
    rate_limit:
      requests_per_minute: 60
      
  - name: "code_execution"
    type: "system"
    description: "Execute code in sandboxed environment"
    capabilities:
      - "python"
      - "javascript"
    constraints:
      max_execution_time: 30
      max_memory_mb: 512
```

**Validation Rules:**
- Each tool MUST have unique `name`
- `type` must be: function, api, system, or custom
- `parameters` follow JSON Schema format
- Security fields validated for tool type

### 4.4 Workflows and Orchestration

Define multi-step agent workflows:

```yaml
workflows:
  data_analysis:
    description: "Analyze data and generate report"
    steps:
      - name: "fetch_data"
        tool: "web_search"
        input:
          query: "${user_query}"
        output: "raw_data"
        
      - name: "analyze"
        model: "primary"
        input: "${raw_data}"
        output: "analysis"
        
      - name: "generate_report"
        model: "primary"
        input:
          analysis: "${analysis}"
          template: "${report_template}"
        output: "final_report"
        
    error_handling:
      on_failure: "retry"
      max_retries: 3
      fallback_workflow: "simple_analysis"
```

**Validation Rules:**
- Steps execute in sequence order
- `tool` or `model` must reference defined resources
- `input` variables must be defined or from previous steps
- Circular dependencies are forbidden

### 4.5 Safety and Constraints

Safety configuration is REQUIRED:

```yaml
safety:
  content_filtering:
    enabled: true
    categories:
      - "hate_speech"
      - "violence"
      - "self_harm"
      - "sexual"
    action: "block"           # block, warn, log
    
  output_validation:
    max_length: 10000
    forbidden_patterns:
      - regex: "API_KEY_[A-Z0-9]+"
        reason: "Prevent API key leakage"
    required_disclaimer: true
    
  rate_limiting:
    requests_per_user_per_hour: 100
    requests_per_ip_per_hour: 1000
    burst_limit: 10
    
  audit:
    log_level: "info"         # debug, info, warn, error
    log_inputs: true
    log_outputs: true
    retention_days: 90
```

**Validation Rules:**
- `safety` section is REQUIRED
- At minimum: content filtering OR output validation
- Audit logging strongly recommended
- PII handling must be documented if logs contain user data

---

## 5. Validation

CAYP validation occurs in three stages:

### 5.1 Syntax Validation
- Valid YAML 1.2 Core Schema
- No forbidden features (anchors, custom tags, etc.)
- Size limits enforced
- Type correctness

### 5.2 Schema Validation
- Structure matches CAYP schema
- Required fields present
- Field types correct
- Cross-references valid

### 5.3 Semantic Validation
- Provider-specific model validation
- Tool parameter compatibility
- Workflow step dependencies
- Security configuration completeness

**Validators MUST:**
- Report all errors, not just first failure
- Provide line numbers and suggestions
- Distinguish between errors and warnings
- Support both strict and permissive modes

---

## 6. Security Considerations

### 6.1 Threat Model

CAYP protects against:
1. **Code Execution:** No custom tags or anchors
2. **DoS Attacks:** Size limits and alias expansion prevention
3. **Data Exfiltration:** No external references, audit logging
4. **Injection Attacks:** Strict type validation, no coercion
5. **Configuration Tampering:** Schema validation, checksums

### 6.2 Security Best Practices

**For CAYP Authors:**
```yaml
# ✅ DO: Use explicit types and quotes
api_key_env: "MY_API_KEY"
timeout: 30

# ❌ DON'T: Store secrets in configs
api_key: "sk-1234567890"  # Use env vars!
```

**For CAYP Parsers:**
- Validate against schema BEFORE executing any config
- Run in sandboxed environment if executing tools
- Log all security-relevant events
- Implement rate limiting for external calls

### 6.3 Secrets Management

CAYP configs MUST NOT contain secrets directly:

```yaml
# ✅ CORRECT - Reference environment variables
authentication:
  api_key_env: "OPENAI_API_KEY"
  
# ✅ CORRECT - Reference secrets manager
authentication:
  secret_manager:
    provider: "aws_secrets_manager"
    secret_id: "prod/agent/api_key"
    
# ❌ INCORRECT - Hardcoded secrets
authentication:
  api_key: "sk-abc123"  # FORBIDDEN
```

---

## 7. Migration from YAML

### 7.1 Automated Migration

CAYP provides migration tools:

```bash
# Convert YAML to CAYP
cayp migrate input.yaml --output config.cayp

# Validate and report issues
cayp validate config.yaml --strict
```

### 7.2 Common Migration Issues

| YAML Pattern | Issue | CAYP Fix |
|-------------|-------|----------|
| `enabled: yes` | Boolean literal | `enabled: true` |
| `version: 1.20` | Sexagesimal | `version: "1.20"` |
| `country: no` | Norway problem | `country: "no"` |
| `&anchor` | Security risk | Remove, duplicate content |
| `<<: *merge` | Merge keys | Flatten into explicit keys |

### 7.3 Compatibility Mode

For gradual migration, CAYP validators can run in compatibility mode:

```yaml
cayp_version: "1.0"
compatibility:
  allow_unquoted_yes_no: true   # Warn instead of error
  allow_implicit_null: true      # Warn instead of error
  migration_target: "2025-12-31" # Deprecation date
```

**Note:** Compatibility mode is for migration only. Production systems should use strict validation.

---

## 8. Tooling Requirements

CAYP implementations MUST provide:

### 8.1 Validator
- Command-line tool: `cayp validate <file>`
- Library API: `cayp.validate(content, strict=True)`
- Exit codes: 0 (valid), 1 (errors), 2 (warnings in strict mode)

### 8.2 Migrator
- YAML → CAYP conversion with safety analysis
- Reports potential issues and breaking changes
- Generates compatibility configuration if needed

### 8.3 Schema Generator
- Generate JSON Schema from CAYP
- Support for custom agentic types
- Documentation generation

### 8.4 IDE Support
- Syntax highlighting
- Auto-completion for known fields
- Inline validation and error messages
- Quick-fixes for common issues

---

## 9. Examples

See `/examples` directory for:
- `valid/`: Compliant CAYP configurations
- `invalid/`: Common mistakes and violations
- `migration/`: Before/after YAML → CAYP conversions

---

## 10. Versioning

CAYP uses semantic versioning: `MAJOR.MINOR.PATCH`

**Version 1.0:** Initial stable release
- Breaking changes: Major version bump
- New features: Minor version bump
- Bug fixes: Patch version bump

**Compatibility:** Parsers MUST support all minor versions within their major version.

```yaml
# Files declare their required version
cayp_version: "1.0"  # Any 1.x parser can read this
```

---

## 11. Governance

CAYP is an open specification:
- **Repository:** https://github.com/cayp-spec/cayp
- **Issues:** GitHub Issues for proposals and bugs
- **RFC Process:** Major changes require RFC + community review
- **License:** MIT (specification), Apache 2.0 (reference implementations)

---

## 12. References

1. YAML 1.2 Specification: https://yaml.org/spec/1.2/spec.html
2. "Norway Problem": https://hitchdev.com/strictyaml/why/implicit-typing-removed/
3. YAML Security: https://www.arp242.net/yaml-config.html
4. JSON Schema: https://json-schema.org/

---

**Document Version:** 1.0.0  
**Last Updated:** November 2025  
**Status:** Draft for Review
