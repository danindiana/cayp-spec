# Contributing to CAYP

Thank you for your interest in contributing to CAYP (Configuration for Agentic Systems - YAML Profile)! This document provides guidelines for contributing to the specification, examples, tools, and documentation.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Specification Changes](#specification-changes)
- [Example Contributions](#example-contributions)
- [Tool Implementation](#tool-implementation)
- [Documentation](#documentation)
- [Development Process](#development-process)
- [Style Guidelines](#style-guidelines)

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment. All contributors are expected to:

- Use welcoming and inclusive language
- Be respectful of differing viewpoints
- Accept constructive criticism gracefully
- Focus on what is best for the community
- Show empathy towards others

### Reporting Issues

Report unacceptable behavior to the project maintainers via GitHub issues.

## How Can I Contribute?

### Reporting Bugs

Before creating a bug report:

1. **Check existing issues** - Your bug may already be reported
2. **Check the specification** - Ensure it's actually a bug, not expected behavior
3. **Gather information** - Include examples, error messages, versions

**Bug Report Template:**

```markdown
**Description:**
Brief description of the issue

**Expected Behavior:**
What should happen

**Actual Behavior:**
What actually happens

**Steps to Reproduce:**
1. Step one
2. Step two
3. ...

**Environment:**
- CAYP Version: 1.0.0
- Tool/Parser: (if applicable)
- OS: (if relevant)

**Additional Context:**
Any other relevant information
```

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues.

**Enhancement Template:**

```markdown
**Problem Statement:**
What problem does this solve?

**Proposed Solution:**
How would you solve it?

**Alternatives Considered:**
What other approaches did you think about?

**Impact:**
- Breaking change? Yes/No
- Affects: Specification/Tools/Examples
- Priority: Low/Medium/High

**Example:**
```yaml
# Example of proposed feature
```
```

### Contributing Code/Documentation

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Commit with clear messages
5. Push to your fork
6. Open a pull request

## Specification Changes

### Types of Changes

#### 1. Clarifications (Patch Version)

**What:** Fixing typos, improving wording, adding examples
**Process:** Submit PR directly
**Review:** 1 maintainer approval

**Example:**
```diff
- Model provider must be valid
+ Model provider must be one of: anthropic, openai, google, cohere
```

#### 2. Backward-Compatible Additions (Minor Version)

**What:** New optional features, extended types
**Process:** Issue → Discussion → RFC (if significant) → PR
**Review:** 2 maintainer approvals + community feedback period

**Example:**
- Adding a new optional field
- Extending allowed provider list
- Adding new tool types

#### 3. Breaking Changes (Major Version)

**What:** Syntax changes, removed features, type changes
**Process:** RFC → Extended discussion → Implementation plan → PR
**Review:** 3 maintainer approvals + public review period

**Example:**
- Changing required fields
- Modifying type system rules
- Removing deprecated features

### RFC Process

For significant changes, write a Request for Comments (RFC):

#### RFC Template

```markdown
# RFC: [Feature Name]

## Summary

One-paragraph explanation of the feature.

## Motivation

Why are we doing this? What use cases does it support? What problems does it solve?

## Detailed Design

Explain the design in enough detail for someone familiar with CAYP to implement it.

### Specification Changes

```yaml
# Example of how it would look
```

### Backward Compatibility

How does this affect existing CAYP files?

### Security Implications

Does this introduce any security concerns?

## Drawbacks

Why should we *not* do this?

## Alternatives

What other designs have been considered? What is the impact of not doing this?

## Open Questions

What parts of the design need to be resolved?

## Implementation

How will this be implemented? Who will implement it?
```

#### RFC Lifecycle

1. **Draft** - Submit as GitHub issue with `rfc` label
2. **Discussion** - Community provides feedback (2-4 weeks)
3. **Final Comment Period** - Last call for objections (1 week)
4. **Accepted/Rejected** - Maintainers make final decision
5. **Implementation** - If accepted, implement and document

## Example Contributions

### Valid Examples

When adding new valid examples:

1. **Create the .cayp file** in `examples/valid/`
2. **Validate it** against the schema
3. **Add comments** explaining key patterns
4. **Document it** in `examples/README.md`

**Example Checklist:**

- [ ] File starts with `cayp_version: "1.0"`
- [ ] All required fields present
- [ ] No ambiguous type coercion
- [ ] No security violations
- [ ] Demonstrates a clear use case
- [ ] Includes inline comments
- [ ] Documented in README

### Invalid Examples

When adding error examples:

1. **Create the .cayp file** in `examples/invalid/`
2. **Add comments** explaining what's wrong
3. **Document** the error type in `examples/README.md`

**Categories:**
- Type coercion errors
- Security violations
- Structural errors
- Semantic errors

### Migration Examples

When adding migration examples:

1. **Create before.yaml** showing problematic YAML
2. **Create after.cayp** showing corrected CAYP
3. **Document** the migration path

## Tool Implementation

### Starting a New Tool

1. **Open an issue** with the `tool-implementation` label
2. **Describe** the tool's purpose and design
3. **Get feedback** from maintainers
4. **Implement** following the guidelines below

### Tool Requirements

#### Validators

**Must Have:**
- [ ] Implement all validation stages (syntax, schema, semantic)
- [ ] Report line numbers for errors
- [ ] Support strict and permissive modes
- [ ] Provide clear error messages
- [ ] Pass all example tests
- [ ] Include README with usage
- [ ] Apache 2.0 license
- [ ] Test coverage > 80%

**Code Example:**
```python
def validate(cayp_content: str, strict: bool = True) -> ValidationResult:
    """
    Validate CAYP content.

    Args:
        cayp_content: CAYP file content
        strict: Enable strict mode (default: True)

    Returns:
        ValidationResult with errors, warnings, and success status
    """
    # Implementation
```

#### Migrators

**Must Have:**
- [ ] Detect YAML issues
- [ ] Generate valid CAYP
- [ ] Warn about manual fixes
- [ ] Generate migration report
- [ ] Handle edge cases gracefully

#### IDE Plugins

**Must Have:**
- [ ] Syntax highlighting for .cayp files
- [ ] Real-time validation
- [ ] Error highlighting
- [ ] Auto-completion
- [ ] Integration with validators

### Testing

All tools must include:

1. **Unit tests** - Core functionality
2. **Integration tests** - Full validation pipeline
3. **Example tests** - All examples/ files
4. **Performance tests** - For validators
5. **Security tests** - No code execution, DoS prevention

### Documentation

Each tool must have:

```
tool-name/
├── README.md          # Installation, usage, examples
├── CHANGELOG.md       # Version history
├── LICENSE            # Apache 2.0
├── docs/
│   ├── api.md         # API documentation
│   └── contributing.md # Tool-specific guidelines
└── tests/
    └── (test files)
```

## Documentation

### Types of Documentation

1. **Specification** - Technical details in `specification/`
2. **Guides** - How-to guides in `docs/`
3. **API docs** - For tools
4. **Examples** - Practical examples in `examples/`

### Documentation Style

- Use clear, concise language
- Include code examples
- Provide both conceptual and practical info
- Link to related documents
- Keep it up-to-date with code changes

### Diagrams

When adding diagrams:

- Use Mermaid for architecture diagrams
- Include textual descriptions for accessibility
- Keep diagrams simple and focused
- Update `docs/ARCHITECTURE.md`

## Development Process

### Branching Strategy

- `main` - Stable releases
- `develop` - Integration branch
- `feature/*` - New features
- `fix/*` - Bug fixes
- `docs/*` - Documentation updates

### Commit Messages

Follow the conventional commits format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation
- `spec` - Specification changes
- `test` - Tests
- `refactor` - Code refactoring
- `chore` - Maintenance

**Examples:**

```
feat(spec): add support for custom tool types

Add extensibility for custom tool types beyond the built-in
function, api, and system types.

Closes #123
```

```
fix(examples): correct type coercion in migration example

The before-yaml.yaml example had an incorrect sexagesimal
example that wasn't actually parsed as sexagesimal.

Fixes #45
```

### Pull Request Process

1. **Update documentation** - Keep docs in sync with changes
2. **Add tests** - For code changes
3. **Update CHANGELOG** - Document your changes
4. **Ensure CI passes** - All checks must pass
5. **Request review** - Tag appropriate reviewers
6. **Address feedback** - Respond to all comments

**PR Template:**

```markdown
## Description

Brief description of changes

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Checklist

- [ ] I have read CONTRIBUTING.md
- [ ] My code follows the style guidelines
- [ ] I have added tests
- [ ] All tests pass
- [ ] I have updated the documentation
- [ ] I have updated CHANGELOG.md

## Related Issues

Closes #123
```

## Style Guidelines

### YAML/CAYP Style

```yaml
# Use 2-space indentation
cayp_version: "1.0"
name: "example"

# Quote all ambiguous values
country: "no"
version: "1.20"

# Use true/false for booleans
enabled: true
debug: false

# Explicit null
optional: null

# Comments above, not inline
# This is the primary model
models:
  primary:
    provider: "anthropic"
```

### Markdown Style

- Use ATX-style headers (`#` not `===`)
- One sentence per line (for diffs)
- Code blocks with language specified
- Lists with `-` not `*`
- Links as references at bottom when repeated

### Code Style

**Python:**
```python
from typing import List, Optional

def validate_cayp(content: str, strict: bool = True) -> bool:
    """
    Validate CAYP content.

    Args:
        content: CAYP file content
        strict: Enable strict mode

    Returns:
        True if valid, False otherwise
    """
    pass
```

**Rust:**
```rust
/// Validate CAYP content
pub fn validate(content: &str, strict: bool) -> Result<(), ValidationError> {
    // Implementation
}
```

**JavaScript:**
```javascript
/**
 * Validate CAYP content
 * @param {string} content - CAYP file content
 * @param {boolean} strict - Enable strict mode
 * @returns {Promise<ValidationResult>}
 */
async function validate(content, strict = true) {
  // Implementation
}
```

## Recognition

Contributors will be recognized in:

- CHANGELOG.md for their contributions
- README.md acknowledgments section
- GitHub contributors list

Significant contributions may lead to:

- Maintainer status
- Decision-making role in RFCs
- Speaking opportunities at events

## Questions?

- **GitHub Issues** - Technical questions
- **GitHub Discussions** - Design discussions
- **Discord** - Real-time chat (coming soon)

## License

By contributing to CAYP, you agree that your contributions will be licensed under:

- **MIT License** for specification and documentation
- **Apache 2.0 License** for reference implementations

See [LICENSE](LICENSE) for details.

---

**Thank you for contributing to CAYP!** 🎉

Together, we're building the future of deterministic, secure configuration for agentic systems.
