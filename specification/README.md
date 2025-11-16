# CAYP Specification

This directory contains the official CAYP (Configuration for Agentic Systems - YAML Profile) specification documents.

## Current Version

**Version 1.0.0** - Draft (November 2025)

- **Document:** [CAYP-SPEC-v1.0.md](CAYP-SPEC-v1.0.md)
- **Status:** Draft for Review
- **Target:** Stable release after community feedback

## What's in the Specification

The CAYP specification defines:

1. **Motivation** - Why YAML needs fixing for agentic systems
2. **Design Principles** - Determinism, security, agentic semantics, production readiness
3. **Technical Specification** - Syntax rules, type system, structural requirements
4. **Agentic Semantics** - Models, tools, workflows, safety configuration
5. **Validation** - Three-stage validation process
6. **Security** - Threat model and best practices
7. **Migration** - YAML to CAYP conversion guide
8. **Tooling Requirements** - Validator, migrator, schema generator, IDE support

## Version Numbering

CAYP follows [Semantic Versioning](https://semver.org/):

```
MAJOR.MINOR.PATCH
```

- **MAJOR** - Breaking changes to syntax or semantics
- **MINOR** - New features, backward compatible
- **PATCH** - Bug fixes, clarifications

### Compatibility Promise

- Parsers implementing version `1.x` MUST accept all `1.y` files where `y >= x`
- Breaking changes require a major version bump
- Deprecation warnings before removal

## Specification History

| Version | Date | Status | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-11-08 | Draft | Initial specification release |

## How to Read the Specification

1. **New to CAYP?** Start with the [README](../README.md) for an overview
2. **Implementing a parser?** Focus on Section 3 (Technical Specification)
3. **Migrating from YAML?** See Section 7 (Migration) and [examples](../examples/)
4. **Security review?** Read Section 6 (Security Considerations)

## Proposing Changes

To propose changes to the specification:

1. Open a GitHub issue with the `spec-proposal` label
2. For major changes, write an RFC (see [CONTRIBUTING.md](../CONTRIBUTING.md))
3. Discuss in community forums
4. Submit a pull request with specification updates

## Related Documents

- **JSON Schema:** [../schemas/cayp-schema-v1.0.json](../schemas/cayp-schema-v1.0.json)
- **Examples:** [../examples/](../examples/)
- **Architecture:** [../docs/ARCHITECTURE.md](../docs/ARCHITECTURE.md)

## License

The CAYP specification is licensed under the MIT License. See [LICENSE](../LICENSE) for details.

Reference implementations may use Apache 2.0 or other compatible licenses.
