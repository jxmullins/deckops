# Security Policy

## Supported Versions

Security updates will be provided for the following versions:

| Version | Supported          |
| ------- | ------------------ |
| latest  | :white_check_mark: |
| < 1.0   | :x:                |

As this project is in early development, we currently only support the latest version. Once stable releases are available, this policy will be updated to reflect long-term support commitments.

## Reporting a Vulnerability

We take the security of DeckOps seriously. If you believe you have found a security vulnerability, please report it to us responsibly.

### How to Report

**Please do NOT report security vulnerabilities through public GitHub issues.**

Instead, please report them via:

- **Email**: happyaccidents@squaresync.dev
- **Subject Line**: [SECURITY] Brief description of the issue

### What to Include

Please include as much of the following information as possible:

- Type of vulnerability (e.g., authentication bypass, code injection, privilege escalation)
- Full paths of source file(s) related to the vulnerability
- Location of the affected source code (tag/branch/commit or direct URL)
- Step-by-step instructions to reproduce the issue
- Proof-of-concept or exploit code (if available)
- Impact of the vulnerability and potential attack scenarios

### Response Timeline

- **Initial Response**: Within 48 hours of report submission
- **Status Update**: Within 7 days with preliminary assessment
- **Fix Timeline**: Depends on severity and complexity
  - Critical: Immediate priority
  - High: Within 7-14 days
  - Medium: Within 30 days
  - Low: Next planned release

### Disclosure Policy

- We request that you give us reasonable time to address the vulnerability before public disclosure
- We will credit you in the security advisory (unless you prefer to remain anonymous)
- Once a fix is released, we will publish a security advisory on GitHub

## Security Considerations

### Docker Socket Access

⚠️ **CRITICAL SECURITY CONSIDERATION**

DeckOps requires access to the Docker socket (`/var/run/docker.sock`) to manage containers. This grants **root-equivalent access** to the host system.

**Important security implications:**

1. **Access Control**: Anyone with access to DeckOps can execute arbitrary commands on your Docker host
2. **Network Exposure**: Never expose DeckOps directly to the public internet without proper authentication
3. **Container Breakout**: Compromised DeckOps instances could potentially escape container isolation

### Deployment Best Practices

#### 1. Network Security

```bash
# ❌ INSECURE - Don't expose directly to the internet
docker run -d -p 0.0.0.0:3000:3000 deckops

# ✅ SECURE - Bind to localhost only
docker run -d -p 127.0.0.1:3000:3000 deckops

# ✅ SECURE - Use with reverse proxy + authentication
# Configure Nginx, Traefik, or Caddy with:
# - HTTPS/TLS encryption
# - Basic authentication or OAuth
# - IP whitelisting
```

#### 2. Authentication

DeckOps does not currently include built-in authentication. You **MUST** implement authentication at the infrastructure level:

- **Reverse Proxy**: Use Nginx, Apache, Traefik, or Caddy with authentication
- **VPN**: Deploy within a private network accessible only via VPN
- **SSH Tunneling**: Access via SSH port forwarding
- **OAuth/SSO**: Integrate authentication at the proxy/gateway level

#### 3. Docker Socket Security

```bash
# Option 1: Use Docker socket proxy (RECOMMENDED)
# Limit API access using tecnativa/docker-socket-proxy
docker run -d \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -p 2375:2375 \
  -e CONTAINERS=1 \
  -e SERVICES=1 \
  -e TASKS=1 \
  tecnativa/docker-socket-proxy

# Then connect DeckOps to the proxy instead of direct socket

# Option 2: Read-only socket (if you only need monitoring)
docker run -d \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  deckops

# Note: Read-only mode will limit functionality
```

#### 4. Container Isolation

```bash
# Run with minimal privileges where possible
docker run -d \
  --security-opt=no-new-privileges \
  --read-only \
  --tmpfs /tmp \
  -v /var/run/docker.sock:/var/run/docker.sock \
  deckops
```

#### 5. Logging and Monitoring

- Enable Docker logging to track all operations
- Monitor DeckOps access logs
- Set up alerts for suspicious container operations
- Regularly audit who has access to DeckOps

### Data Security

- **Compose Files**: May contain sensitive data (credentials, API keys)
- **Environment Variables**: Often contain secrets
- **Volume Mounts**: May expose sensitive host directories

**Recommendations:**
- Use Docker secrets or external secret management (Vault, AWS Secrets Manager)
- Avoid storing credentials in compose files
- Implement file-level access controls
- Regularly rotate credentials

### Development Security

If running DeckOps in development mode:

- Never use development builds in production
- Development servers may have verbose error messages exposing internals
- Hot-reload features may introduce vulnerabilities
- Ensure development dependencies are not included in production builds

## Security Features Roadmap

We are actively working on improving DeckOps security:

- [ ] Built-in authentication system
- [ ] Role-based access control (RBAC)
- [ ] Audit logging for all operations
- [ ] Support for Docker socket proxy
- [ ] Security scanning integration
- [ ] Two-factor authentication (2FA)
- [ ] API key management
- [ ] Session management and timeout

## Third-Party Dependencies

We regularly monitor our dependencies for known vulnerabilities:

- Go dependencies: Using `go mod` and scanning with `govulncheck`
- npm dependencies: Using `npm audit` for frontend packages
- Base container images: Regularly updated to latest secure versions

## Security Scanning

This repository uses:

- **Dependabot**: Automated dependency updates and security alerts
- **CodeQL**: Static analysis for security vulnerabilities
- **Container Scanning**: Docker image vulnerability scanning

## Responsible Disclosure Examples

Examples of security issues we want to know about:

✅ **Please Report:**
- Authentication bypass vulnerabilities
- Container escape possibilities
- Code injection vulnerabilities (XSS, SQL injection, command injection)
- Privilege escalation vulnerabilities
- Information disclosure (sensitive data leakage)
- CSRF vulnerabilities
- Path traversal vulnerabilities
- Docker API abuse possibilities

❌ **Not Security Issues:**
- Bugs without security impact
- Feature requests
- Best practice recommendations (open a regular issue instead)

## Additional Resources

- [Docker Security Best Practices](https://docs.docker.com/engine/security/)
- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
- [CIS Docker Benchmark](https://www.cisecurity.org/benchmark/docker)

## Questions?

If you have questions about this security policy or DeckOps security in general:

- Open a GitHub Discussion (for non-sensitive questions)
- Email: happyaccidents@squaresync.dev (for private inquiries)

---

**Last Updated**: 2026-01-14

This security policy is subject to change. The version in the `main` branch of the official repository is authoritative.
