# Contributing to BitVault Pro

Thank you for your interest in contributing to BitVault Pro! This document provides guidelines and information for contributors.

## 🤝 How to Contribute

### Reporting Issues

- Use the GitHub issue tracker
- Provide detailed reproduction steps
- Include relevant error messages and logs
- Tag issues appropriately (bug, enhancement, question)

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch from `main`
3. Make your changes with tests
4. Ensure all tests pass
5. Submit a pull request

## 🛠️ Development Setup

### Prerequisites

```bash
# Install Node.js 18+
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install Clarinet
curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/hirosystems/clarinet/main/install.sh | sh
```

### Local Development

```bash
# Clone and setup
git clone https://github.com/hammedibr/bitvault.git
cd bitvault
npm install

# Run tests
npm test

# Check contracts
clarinet check
```

## 📝 Code Standards

### Clarity Best Practices

- Use descriptive function and variable names
- Add comprehensive comments
- Handle all error cases
- Use consistent formatting
- Follow the existing code style

### Testing Requirements

- Write unit tests for all new functions
- Maintain test coverage above 80%
- Test both success and failure scenarios
- Use meaningful test descriptions

### Documentation

- Update README.md for new features
- Add inline code documentation
- Update API reference for public functions
- Include usage examples

## 🔍 Code Review Process

1. All pull requests require review
2. Reviewers check for:
   - Code quality and standards
   - Test coverage
   - Security considerations
   - Documentation updates
3. Address review feedback promptly
4. Maintainers will merge approved PRs

## 🏷️ Issue Labels

- `bug`: Something isn't working
- `enhancement`: New feature or request
- `documentation`: Improvements to docs
- `good first issue`: Good for newcomers
- `help wanted`: Extra attention needed
- `security`: Security-related issues

## 📋 Commit Message Format

```
type(scope): description

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `test`, `refactor`, `chore`

Example:

```
feat(staking): add time-lock multiplier calculation

Add support for 30-day and 60-day lock periods with 
enhanced reward multipliers for committed stakers.

Closes #123
```

## 🎯 Priority Areas

We especially welcome contributions in:

- Test coverage improvements
- Documentation enhancements
- Security auditing
- Performance optimizations
- User experience improvements
