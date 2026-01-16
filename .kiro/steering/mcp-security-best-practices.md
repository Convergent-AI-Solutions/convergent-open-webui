# MCP & Third-Party Integration Security

Security best practices for Model Context Protocol (MCP) servers and third-party integrations in Kiro (or any IDE).

## Security Risks

MCP servers and integrations connect to external services and execute third-party code. Key risks:

| Risk | Description |
|------|-------------|
| **Credential Exposure** | API keys and tokens may be required and potentially leaked |
| **External Code Execution** | Third-party code runs outside IDE sandboxes |
| **Data Transmission** | Information flows to external services |
| **Supply Chain** | Malicious or compromised integration sources |

**Before using any integration**: Review source code and verify it comes from a trusted source.

## Credential Security

### Mandatory Practices

- **NEVER** commit tokens or API keys to version control
- Use environment variables instead of hardcoding values
- Create tokens with minimal required permissions
- Use fine-grained tokens (e.g., GitHub fine-grained PATs over classic tokens)
- Limit scope to only required resources
- Rotate credentials regularly

### Environment Variables

**Correct approach:**
```json
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

```bash
# Set in shell (don't commit this)
export GITHUB_TOKEN=your-token-value
```

### Approved Variables

Kiro (or any IDE) may require explicit approval for environment variable expansion:

1. Security warnings appear for unapproved variables
2. Approve only variables you explicitly need
3. This prevents integrations from accessing arbitrary system variables

### File Permissions

```bash
# Restrict config file access
chmod 600 ~/.kiro/settings/mcp.json
chmod 600 .kiro/settings/mcp.json
```

## Tool Approval

### Review Process

Before approving any tool request:

1. **Check parameters** - What data is being passed?
2. **Understand the action** - What will this tool do?
3. **Verify the source** - Is this from a trusted integration?
4. **Match to task** - Does this align with your current work?

**Deny suspicious requests that don't match your current task.**

### Auto-Approval Criteria

Only auto-approve tools that meet ALL criteria:

- ✅ Read-only (no write access to sensitive systems)
- ✅ Trusted source with verified code
- ✅ Frequently used in your workflow
- ✅ Limited scope

```json
{
  "mcpServers": {
    "aws-docs": {
      "autoApprove": [
        "search_documentation",
        "read_documentation"
      ]
    }
  }
}
```

## Workspace Isolation

Use project-level configurations to isolate integrations:

```
project-a/.kiro/settings/mcp.json  # Project A only
project-b/.kiro/settings/mcp.json  # Project B only
```

**Benefits:**
- Integrations only active in relevant projects
- Tokens isolated between projects
- Security risks contained per workspace

## Monitoring & Auditing

### Regular Reviews

- Check integration logs for unusual activity (Kiro: Output tab → "Kiro - MCP Logs")
- Review auto-approved tools periodically
- Remove approvals for unused tools
- Monitor for unexpected behavior

### Audit Checklist

- [ ] Review all auto-approved tools quarterly
- [ ] Check for unused integrations
- [ ] Verify token permissions are still minimal
- [ ] Rotate credentials on schedule

## Incident Response

**If you suspect a security issue:**

1. **Immediately:**
   - Disable the integration
   - Revoke associated tokens/API keys
   - Check connected services for unauthorized activity

2. **Follow-up:**
   - Report to integration maintainer
   - Document the incident
   - Update security practices

## Additional Measures

### Network
- Firewall outbound connections from integrations
- Use VPN for sensitive connections
- Monitor integration network traffic

### System
- Keep systems patched
- Run integrations with minimal privileges
- Audit and remove unused integrations regularly

## Team Guidelines

| Role | Responsibilities |
|------|------------------|
| **All Members** | Never share credentials; report suspicious behavior; follow least privilege |
| **Project Leads** | Maintain approved integration lists; conduct security reviews; train team |
| **Admins** | Monitor org-wide usage; centralize logging; establish incident procedures |

---

*Follow these practices to maintain security when using MCP servers and third-party integrations in Kiro (or any IDE).*