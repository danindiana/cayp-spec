# CAYP Tools

This directory will contain reference implementations of CAYP validators, migrators, and other tooling.

## Current Status

⏳ **Phase 1 (Current):** Specification development and review
⏳ **Phase 2 (Next):** Reference implementation development

## Planned Tools

### 1. Validators

Validate CAYP files against the specification.

#### Python Validator
```bash
# Installation (future)
pip install cayp-validator

# Usage
cayp validate config.cayp --strict
cayp validate config.cayp --output report.json
```

**Features:**
- ✅ Syntax validation (YAML 1.2 compliance)
- ✅ Schema validation (structure correctness)
- ✅ Semantic validation (cross-references, types)
- ✅ Security checks (no hardcoded secrets)
- ✅ Clear error messages with line numbers
- ✅ JSON/YAML output formats

**Target:** Python 3.8+
**License:** Apache 2.0

#### Rust Validator
```bash
# Installation (future)
cargo install cayp-validator

# Usage
cayp-validate config.cayp --strict
```

**Features:**
- ⚡ High performance (for CI/CD pipelines)
- 🔒 Memory safe
- 📦 Single binary distribution
- 🌐 WASM support for web tools

**Target:** Rust 1.70+
**License:** Apache 2.0

#### JavaScript Validator
```bash
# Installation (future)
npm install @cayp/validator

# Usage
import { validate } from '@cayp/validator';
const result = await validate(caypContent);
```

**Features:**
- 🌐 Browser and Node.js support
- 🔧 IDE integration ready
- 📝 TypeScript definitions
- ⚛️ React/Vue component support

**Target:** ES2020+, TypeScript 4.5+
**License:** Apache 2.0

### 2. Migrator

Convert legacy YAML to CAYP format.

```bash
# Usage (future)
cayp migrate legacy.yaml --output config.cayp
cayp migrate legacy.yaml --report migration-report.json
```

**Features:**
- 🔄 Automatic conversion of common patterns
- 🔍 Security issue detection
- ⚠️ Warning for manual fixes needed
- 📊 Migration compatibility report
- 💾 Backup original files

**Conversion Capabilities:**
- Type coercion fixes (`no` → `"no"`)
- Boolean normalization (`yes` → `true`)
- Anchor/alias expansion
- Secret extraction to environment variables
- Required field injection
- Safety configuration templates

### 3. Schema Generator

Generate JSON Schema from CAYP configurations.

```bash
# Usage (future)
cayp schema generate config.cayp --output schema.json
cayp schema validate config.cayp --schema custom-schema.json
```

**Features:**
- 📐 Auto-generate schemas from examples
- 🔧 Custom type extensions
- 📚 Documentation generation
- 🔗 Schema composition support

### 4. IDE Plugins

Enhance developer experience with IDE support.

#### VS Code Extension

**Features:**
- 🎨 Syntax highlighting
- 💡 Auto-completion
- 🔍 Real-time validation
- 🛠️ Quick fixes for common errors
- 📖 Inline documentation

**Marketplace:** `cayp-lang` (future)

#### IntelliJ/PyCharm Plugin

**Features:**
- Language support for .cayp files
- Schema-aware editing
- Refactoring support
- Integration with validators

### 5. CI/CD Integrations

#### GitHub Actions

```yaml
# .github/workflows/validate-cayp.yml
name: Validate CAYP
on: [push, pull_request]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: cayp/validate-action@v1
        with:
          files: 'configs/**/*.cayp'
          strict: true
```

#### GitLab CI

```yaml
# .gitlab-ci.yml
validate-cayp:
  image: cayp/validator:latest
  script:
    - cayp validate configs/**/*.cayp --strict
```

#### Pre-commit Hook

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/cayp-spec/cayp-validator
    rev: v1.0.0
    hooks:
      - id: cayp-validate
        files: \.cayp$
```

## Implementation Roadmap

### Phase 2.1: Core Validators (Q1 2026)

- [ ] Python validator (reference implementation)
- [ ] Core validation logic
- [ ] Error reporting framework
- [ ] Unit test suite
- [ ] Documentation

### Phase 2.2: High-Performance Validator (Q2 2026)

- [ ] Rust validator
- [ ] Performance benchmarks
- [ ] WASM compilation
- [ ] Binary distributions

### Phase 2.3: Web Ecosystem (Q2 2026)

- [ ] JavaScript/TypeScript validator
- [ ] npm package
- [ ] Browser bundle
- [ ] Online validator tool

### Phase 2.4: Migration Tools (Q3 2026)

- [ ] YAML analyzer
- [ ] Automatic migration engine
- [ ] Security scanner
- [ ] Migration report generator

### Phase 3: Ecosystem Tools (Q4 2026)

- [ ] VS Code extension
- [ ] IntelliJ plugin
- [ ] GitHub Actions
- [ ] Pre-commit hooks
- [ ] Online playground

## Contributing

We welcome contributions to CAYP tooling!

### Getting Started

1. **Choose a tool** - Pick from the roadmap above
2. **Open an issue** - Discuss your implementation plan
3. **Follow the spec** - Use [specification/](../specification/) as reference
4. **Write tests** - Use [examples/](../examples/) for test cases
5. **Submit PR** - See [CONTRIBUTING.md](../CONTRIBUTING.md)

### Implementation Guidelines

#### For Validators

**Must:**
- Implement all validation stages (syntax, schema, semantic)
- Report line numbers for errors
- Support strict and permissive modes
- Provide clear error messages with suggestions

**Should:**
- Support multiple output formats (text, JSON, YAML)
- Be fast enough for CI/CD (< 100ms for typical configs)
- Have comprehensive test coverage (> 90%)
- Include usage examples and documentation

**Nice to have:**
- Auto-fix capabilities
- Integration with popular editors
- Performance benchmarks
- Localization support

#### For Migrators

**Must:**
- Detect common YAML issues
- Generate valid CAYP output
- Warn about manual fixes needed
- Preserve comments where possible

**Should:**
- Generate migration reports
- Support batch processing
- Handle multiple YAML versions
- Extract secrets to environment variables

### Language-Specific Conventions

**Python:**
- Use type hints (Python 3.8+)
- Follow PEP 8
- Use pytest for testing
- Support pip installation

**Rust:**
- Follow Rust API guidelines
- Use cargo for building
- Support WASM target
- Publish to crates.io

**JavaScript:**
- Provide TypeScript types
- Support CommonJS and ESM
- Follow semantic versioning
- Publish to npm

## Resources

- **Specification:** [../specification/CAYP-SPEC-v1.0.md](../specification/CAYP-SPEC-v1.0.md)
- **Schema:** [../schemas/cayp-schema-v1.0.json](../schemas/cayp-schema-v1.0.json)
- **Examples:** [../examples/](../examples/)
- **Contributing:** [../CONTRIBUTING.md](../CONTRIBUTING.md)

## Testing Your Implementation

### Validation Test Suite

Your validator should pass all tests in [examples/](../examples/):

```bash
# Valid examples should pass
✅ examples/valid/comprehensive-agent.cayp

# Invalid examples should fail with appropriate errors
❌ examples/invalid/type-coercion-errors.cayp
❌ examples/invalid/security-violations.cayp
❌ examples/invalid/structural-errors.cayp
```

### Performance Benchmarks

Target performance for validators:

| File Size | Parse Time | Memory Usage |
|-----------|------------|--------------|
| < 10 KB | < 10 ms | < 5 MB |
| < 100 KB | < 100 ms | < 50 MB |
| < 1 MB | < 1 s | < 100 MB |

### Compatibility Testing

Ensure compatibility with:
- YAML 1.2 Core Schema parsers
- JSON Schema validators
- Major operating systems (Linux, macOS, Windows)

## Support

Need help implementing a tool?

- **GitHub Issues:** Technical questions
- **Discussions:** Design decisions
- **Discord:** Real-time chat (coming soon)

---

**Last Updated:** 2025-11-16
**Next Review:** After Phase 2 kickoff
