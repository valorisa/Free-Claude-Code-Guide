# Development Guide

This document provides instructions for developers who want to contribute to or modify this guide.

## Project Structure

```text
Free-Claude-Code-Guide/
├── .github/
│   ├── workflows/          # CI/CD pipelines
│   │   ├── ci.yml         # Main CI workflow
│   │   └── markdownlint.yml
│   ├── ISSUE_TEMPLATE/    # Issue templates
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   ├── pull_request_template.md
│   ├── markdown-link-check-config.json
│   └── dependabot.yml
├── .editorconfig          # Editor configuration
├── .gitattributes         # Git attributes
├── .gitignore            # Git ignore rules
├── .markdownlint.json    # Markdown linting rules
├── CODEOWNERS            # Code ownership
├── CODE_OF_CONDUCT.md    # Community guidelines
├── CONTRIBUTING.md       # Contribution guidelines
├── CHANGELOG.md          # Version history
├── SECURITY.md           # Security policy
├── LICENSE               # MIT License
├── README.md             # Main documentation
└── DEVELOPMENT.md        # This file
```

## Local Development Setup

### Prerequisites

- Git
- Node.js (pour markdownlint-cli)
- Python 3 (pour codespell)

### Installing Tools

```bash
# Install markdownlint-cli globally
npm install -g markdownlint-cli

# Install markdown-link-check globally
npm install -g markdown-link-check

# Install codespell for spell checking
pip install codespell
```

### Linting Markdown Locally

Before pushing changes, run the linters locally:

```bash
# Check markdown style
markdownlint '**/*.md' --config .markdownlint.json

# Check for broken links
find . -name "*.md" -not -path "./.git/*" -exec markdown-link-check -q {} \;

# Check spelling
codespell --lang fr,en --skip ".git,node_modules"
```

## Making Changes

### 1. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

### 2. Make Your Changes

Edit the relevant files. Remember:

- Write in French (primary language)
- Be explicit and verbose in explanations
- Include examples for every configuration option
- Explain "why" not just "how"

### 3. Test Your Changes

```bash
# Run all checks locally
markdownlint '**/*.md' --config .markdownlint.json
codespell --lang fr,en --skip ".git,node_modules"
```

### 4. Commit Your Changes

Follow the conventional commit format:

```bash
git add .
git commit -m "feat: add section about proxy configuration"
# or
git commit -m "fix: correct NVIDIA NIM model identifier"
# or
git commit -m "docs: update Windows Enterprise instructions"
```

### 5. Push and Create Pull Request

```bash
git push origin your-branch-name
```

Then create a Pull Request on GitHub using the template.

## CI/CD Pipeline

The CI pipeline runs automatically on:

- Pushes to `main` branch
- Pull requests targeting `main` branch

### Jobs

1. **markdown-lint**: Checks Markdown style using markdownlint
2. **validate-links**: Checks for broken links in documentation
3. **check-spelling**: Spell checks using codespell (French/English)
4. **build**: Verifies all standard files exist

## Common Tasks

### Adding a New Section to README

1. Edit `README.md`
2. Add your section with proper Markdown formatting
3. Include code examples with language identifiers
4. Run `markdownlint README.md` to check style
5. Commit with `docs: add new section about...`

### Updating Configuration Examples

1. Edit the relevant section in `README.md`
2. Ensure code blocks have language identifiers (e.g., ```bash)
3. Test commands mentally or in a sandbox
4. Update `CHANGELOG.md` with your changes
5. Commit with `fix: update configuration example for...`

### Adding a New Platform

1. Add a new subsection under the platform section in `README.md`
2. Include all relevant commands and notes
3. Test the instructions if possible
4. Commit with `feat: add support for [platform]...`

## Tips

- Keep lines under 120 characters in Markdown files
- Use tables for structured data
- Use fenced code blocks with language identifiers
- Explain technical concepts thoroughly
- Include "why" something is configured a certain way
- Use emojis sparingly and only when relevant

## Getting Help

If you need help:

1. Check existing issues on GitHub
2. Review the CONTRIBUTING.md guidelines
3. Create a new issue with the "question" label

Thank you for contributing!
