# Changelog

All notable changes to the CAYP (Configuration for Agentic Systems - YAML Profile) specification and repository will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned

- Reference validator implementation (Python)
- Reference validator implementation (Rust)
- YAML to CAYP migration tool
- VS Code extension
- Online validator playground

## [1.0.0] - 2025-11-16

### Added - Repository Organization

- Organized repository into logical directory structure:
  - `specification/` - Core CAYP specification documents
  - `schemas/` - JSON Schema for validation
  - `examples/` - Organized into valid/, invalid/, and migration/ subdirectories
  - `docs/` - Centralized documentation
  - `tools/` - Placeholder for future implementations
- Created comprehensive architecture documentation with 9 Mermaid diagrams
- Added `.gitignore` for common development artifacts
- Created README files for each major directory:
  - `specification/README.md` - Specification versioning guide
  - `examples/README.md` - Example usage guide
  - `tools/README.md` - Implementation roadmap
- Added `CONTRIBUTING.md` with detailed contribution guidelines
- Added `LICENSE` with dual-licensing structure (MIT + Apache 2.0 + CC0)
- Added `CHANGELOG.md` (this file)

### Added - Documentation

- `docs/ARCHITECTURE.md` - Visual documentation with diagrams:
  - Repository structure diagram
  - CAYP validation workflow
  - Agentic systems integration
  - YAML to CAYP migration process
  - Type system visualization
  - Development workflow phases
  - Safety architecture
  - Multi-step workflow sequence
  - Format comparison (YAML vs JSON vs CAYP)
- Updated `README.md` with:
  - Links to new documentation structure
  - Visual guide reference
  - Corrected file paths

### Added - GitHub Templates

- Issue templates for bug reports, feature requests, and RFCs
- Pull request template with checklist
- GitHub Actions workflows for CI/CD

## [1.0.0-draft] - 2025-11-08

### Added - Initial Specification

- Core CAYP specification (`CAYP-SPEC-v1.0.md`)
  - Motivation and problem statement
  - Design principles (determinism, security, agentic semantics, production readiness)
  - Technical specification (syntax, type system, structural rules)
  - Agentic semantics (models, tools, workflows, safety)
  - Validation stages (syntax, schema, semantic)
  - Security considerations
  - Migration guide from YAML
  - Tooling requirements

### Added - Schema

- JSON Schema for CAYP validation (`cayp-schema-v1.0.json`)
  - Structural validation
  - Type validation
  - Required field validation
  - Cross-reference validation

### Added - Examples

- Valid example: `comprehensive-agent.cayp`
  - Demonstrates all major CAYP features
  - Production-ready agent configuration
  - Comments explaining key patterns

- Invalid examples:
  - `type-coercion-errors.cayp` - Common type mistakes
  - `security-violations.cayp` - Security anti-patterns
  - `structural-errors.cayp` - Schema violations

- Migration examples:
  - `before-yaml.yaml` - Problematic YAML configuration
  - `after-cayp.cayp` - Corrected CAYP configuration

### Added - Documentation

- `README.md` - Project overview and quick start
- `MERGE_SUMMARY.md` - Branch merge documentation
- `PHASE_1_REVIEW.md` - Phase 1 implementation review
- `SESSION_SUMMARY.md` - Development session notes
- `WORK_SUMMARY_2025-11-08.md` - Daily work log

### Added - Development Environment

- VS Code workspace configuration
  - Custom settings for YAML editing
  - Task definitions
  - Recommended extensions
  - Launch configurations

## Version History

### Version Numbering

CAYP follows [Semantic Versioning](https://semver.org/):

- **MAJOR version** - Incompatible changes (breaking changes)
- **MINOR version** - Backward-compatible functionality additions
- **PATCH version** - Backward-compatible bug fixes

### Compatibility

- Parsers implementing version `1.x` MUST accept all `1.y` files where `y >= x`
- Files declare required version: `cayp_version: "1.0"`
- Breaking changes require major version bump and deprecation period

## Upcoming Milestones

### Phase 2: Reference Implementations (Q1-Q2 2026)

- [ ] Python validator library
- [ ] Rust validator with WASM support
- [ ] JavaScript/TypeScript validator
- [ ] Migration tools (YAML → CAYP)
- [ ] Command-line tools

### Phase 3: Ecosystem Tools (Q3-Q4 2026)

- [ ] VS Code extension
- [ ] IntelliJ/PyCharm plugin
- [ ] GitHub Actions integration
- [ ] GitLab CI integration
- [ ] Pre-commit hooks
- [ ] Online playground

### Phase 4: Standardization (2027)

- [ ] RFC publication
- [ ] Standards body submission
- [ ] Industry partnerships
- [ ] Multi-language ecosystem growth

## Migration Guide

### From Pre-1.0 Drafts

If you used early CAYP drafts:

1. Ensure `cayp_version: "1.0"` is present
2. Verify all required fields are included
3. Check safety configuration completeness
4. Validate against updated schema

### From YAML

See the [migration examples](examples/migration/) for detailed conversion guidance.

Key changes:
- Quote ambiguous values: `country: "no"` not `country: no`
- Use `true`/`false` for booleans, not `yes`/`no`
- Add required CAYP metadata fields
- Add safety configuration section
- Remove anchors/aliases (expand content)
- Extract secrets to environment variables

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for:
- How to propose specification changes
- RFC process for major features
- Example contribution guidelines
- Tool implementation requirements

## Links

- **Repository:** https://github.com/cayp-spec/cayp
- **Specification:** [specification/CAYP-SPEC-v1.0.md](specification/CAYP-SPEC-v1.0.md)
- **Architecture:** [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- **Examples:** [examples/](examples/)
- **Issues:** GitHub Issues
- **Discussions:** GitHub Discussions

---

**Note:** This changelog covers both specification changes and repository/tooling changes. For tool-specific changelogs, see the respective tool directories.
