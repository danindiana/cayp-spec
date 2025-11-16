# CAYP Examples

This directory contains example CAYP configurations to help you understand the format and common patterns.

## Directory Structure

```
examples/
├── valid/          # Working examples that pass validation
├── invalid/        # Examples showing common errors
└── migration/      # Before/after YAML → CAYP conversions
```

## Valid Examples

### [comprehensive-agent.cayp](valid/comprehensive-agent.cayp)

A full-featured example demonstrating:
- Multiple model configurations with fallbacks
- Tool definitions with authentication
- Multi-step workflows with error handling
- Comprehensive safety configuration
- Environment variable usage
- Rate limiting and audit logging

**Use this as a template** for production agent configurations.

## Invalid Examples

Learn from common mistakes:

### [type-coercion-errors.cayp](invalid/type-coercion-errors.cayp)
- Unquoted ambiguous values (`no`, `yes`, `on`, `off`)
- Sexagesimal number confusion (`1:20` → `80`)
- Implicit null values
- Legacy boolean syntax

### [security-violations.cayp](invalid/security-violations.cayp)
- Hardcoded API keys and secrets
- Use of forbidden YAML features (anchors, aliases)
- Custom tags and merge keys
- External references

### [structural-errors.cayp](invalid/structural-errors.cayp)
- Missing required fields
- Invalid cross-references
- Incorrect type usage
- Schema violations

## Migration Examples

### [before-yaml.yaml](migration/before-yaml.yaml) → [after-cayp.cayp](migration/after-cayp.cayp)

Side-by-side comparison showing:
1. **Type coercion fixes** - Quoting ambiguous values
2. **Security improvements** - Removing anchors, extracting secrets
3. **Required fields** - Adding CAYP metadata
4. **Safety config** - Adding production-ready safety controls

## How to Use These Examples

### 1. Learning CAYP

Start with the valid example:
```bash
# Read the comprehensive example
cat examples/valid/comprehensive-agent.cayp

# Compare with specification
less specification/CAYP-SPEC-v1.0.md
```

### 2. Understanding Errors

Study the invalid examples to avoid common pitfalls:
```bash
# See what NOT to do
cat examples/invalid/type-coercion-errors.cayp
cat examples/invalid/security-violations.cayp
```

### 3. Migrating from YAML

Use the migration example as a guide:
```bash
# Compare before and after
diff examples/migration/before-yaml.yaml examples/migration/after-cayp.cayp
```

### 4. Validating Your Config

Once validators are available:
```bash
# Validate against schema
cayp validate your-config.cayp --strict

# Run migration analysis
cayp migrate your-legacy.yaml --output your-config.cayp --report
```

## Common Patterns

### Minimal CAYP Config

```yaml
cayp_version: "1.0"
name: "simple-agent"
description: "Minimal working example"
created: "2025-11-16"

models:
  primary:
    provider: "anthropic"
    model: "claude-sonnet-4"

safety:
  content_filtering:
    enabled: true
    action: "block"
  audit:
    log_level: "info"
```

### Environment Variables

```yaml
authentication:
  api_key_env: "OPENAI_API_KEY"  # ✅ Read from environment
  # api_key: "sk-123"            # ❌ NEVER hardcode secrets
```

### Model Fallbacks

```yaml
models:
  primary:
    provider: "anthropic"
    model: "claude-sonnet-4"
    fallback: "backup"

  backup:
    provider: "openai"
    model: "gpt-4"
```

### Multi-Step Workflows

```yaml
workflows:
  analyze_and_report:
    steps:
      - name: "fetch"
        tool: "data_fetch"
        output: "raw_data"
      - name: "analyze"
        model: "primary"
        input: "${raw_data}"
        output: "analysis"
      - name: "report"
        model: "primary"
        input: "${analysis}"
        output: "final_report"
```

## Testing Your Configurations

### Manual Validation Checklist

- [ ] File starts with `cayp_version: "1.0"`
- [ ] Required fields present: `name`, `description`, `created`
- [ ] All ambiguous strings are quoted
- [ ] Booleans use `true`/`false` only
- [ ] No anchors (`&`) or aliases (`*`)
- [ ] No hardcoded secrets
- [ ] `safety` section is complete
- [ ] Model/tool references are valid

### Schema Validation (Future)

```bash
# When validators are available
cayp validate config.cayp

# Expected output for valid config:
✅ Syntax validation passed
✅ Schema validation passed
✅ Semantic validation passed
✅ Security checks passed
```

## Contributing Examples

Have a useful CAYP pattern? We'd love to include it!

1. Ensure your example validates against the schema
2. Add clear comments explaining the pattern
3. Submit a pull request with:
   - The example file
   - Description of the use case
   - Any special considerations

See [CONTRIBUTING.md](../CONTRIBUTING.md) for details.

## Need Help?

- **Specification:** [../specification/CAYP-SPEC-v1.0.md](../specification/CAYP-SPEC-v1.0.md)
- **Architecture:** [../docs/ARCHITECTURE.md](../docs/ARCHITECTURE.md)
- **Issues:** Open a GitHub issue
- **Discussions:** Join community forums (coming soon)

## Example Versioning

These examples are compatible with **CAYP v1.0.0**.

When the specification is updated, examples will be versioned accordingly:
- Major spec changes → New example versions
- Minor spec changes → Examples updated in place
- Deprecated features → Migration guides provided
