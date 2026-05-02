# Security Policy

## Reporting a Vulnerability

We take the security of Free-Claude-Code-Guide seriously. If you believe you have found a security vulnerability, please report it to us as described below.

### How to Report a Security Vulnerability?

If you find a security vulnerability, do **NOT** open a public issue. Instead, please report it privately by emailing us at [valorisa@example.com].

Please include the following information in your report:

* Type of vulnerability (e.g., API key exposure, injection, etc.)
* Full path to the affected file(s)
* Steps to reproduce the vulnerability
* Potential impact of the vulnerability
* Suggested fix (if any)

### What to Expect

After you submit a vulnerability report:

1. **Acknowledgment**: We will acknowledge receipt within 48 hours
2. **Investigation**: We will investigate and determine the scope and severity
3. **Communication**: We will keep you informed of our progress
4. **Resolution**: We will work on a fix and prepare a security advisory
5. **Disclosure**: We will coordinate public disclosure with you

## Security Best Practices for This Project

### API Keys and Secrets

⚠️ **NEVER** commit the following to this repository:

* `.env` files containing API keys
* NVIDIA NIM API keys
* OpenRouter API keys
* Any authentication tokens

The `.gitignore` file is configured to prevent accidental commits of `.env` files, but always double-check before pushing.

### Safe Configuration

When documenting configuration examples:

* Use placeholder values like `nvapi-votre-clé` or `sk-or-votre-clé`
* Never use real API keys in documentation
* Use examples that clearly indicate they need to be replaced

### Local Development

* Keep your local `.env` file secure and private
* Don't share screenshots that might contain API keys
* Use environment variables rather than hardcoding values

## Supported Versions

This project is a documentation guide. Security updates apply to:

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Scope

This security policy covers:

* The documentation in this repository
* Configuration examples provided
* CI/CD workflows

This policy does **NOT** cover:

* The upstream `free-claude-code` project (https://github.com/Alishahryar1/free-claude-code)
* NVIDIA NIM services
* Claude Code CLI software

## Attribution

Some parts of this security policy were adapted from the [GitHub Security Policy template](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository).
