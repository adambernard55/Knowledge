# MCP Security and Compliance

### Safeguarding Agentic AI Integrations

---

## Overview

Security and regulatory compliance are the backbone of **Model Context Protocol (MCP)** adoption.  
As agents gain the ability to read, write, and act within enterprise systems, MCP introduces both **powerful capabilities** and **new security responsibilities**.

This reference explores how organizations should secure the full MCP stack—from endpoint design and authentication to governance, auditing, and regulatory compliance—with practical guidance aligned to the **March 2025 MCP Security Best Practices specification**.

---

## 1. Security in the MCP Architecture

MCP enforces **security by design** through architectural boundaries:

|Role|Exposure Surface|Security Responsibility|
|---|---|---|
|**Host**|User interface and session memory|Consent management · session isolation|
|**Client**|JSON‑RPC transport and routing logic|Credential storage · policy enforcement per server|
|**Server**|Tool execution and data access|Authentication · authorization · logging · sandboxing|

Each component must **act as a security boundary** and validate all protocol exchanges independently.

---

## 2. The MCP Security Stack

|Layer|Threat Scope|Required Controls|
|---|---|---|
|**Transport**|Interception · spoofing · downgrade attacks|HTTPS w/ TLS 1.3 · HSTS  · origin check · CSRF prevention|
|**Authentication**|Token reuse · leaked secrets|OAuth 2.1 + PKCE  · short‑lived access tokens  · refresh flow|
|**Authorization & Scopes**|Escalated permissions|Per‑tool scope policies  · consent screens  · role‑based ACLs|
|**Execution Environment**|Code injection · sandbox escape|Containerization  · least privilege  · resource quota limits|
|**Logging and Audit**|Sensitive data exposure|Structured log masking  · token redaction  · hash PII fields|

**Principle:** _Every MCP request must fail securely_ — unrecognized or unauthenticated calls are silently dropped and logged for later analysis.

---

## 3. Authentication and Authorization

### 3.1 OAuth 2.1 + PKCE (Spec March 2025)

- HTTP servers must use the OAuth 2.1 authorization‑code flow with PKCE.
- Tokens expire in ≤ 3600 seconds (default).
- Clients can request specific tool‑scopes: `database:read`, `storage:write`, etc.

**Token Structure:**

```json
{
  "sub": "user-1234",
  "scope": "database:read storage:read",
  "aud": "supabase-mcp",
  "iat": 1734176000,
  "exp": 1734179600
}
```

**Best Practices**

- Never embed personal tokens in tool definitions.
- Support revocation and rotation via `.well-known/openid-configuration`.
- Verify signatures with JSON Web Keys (JWK URI) on every request.

---

### 3.2 Fine-Grained Scopes & Policy Mapping

|Operation|Recommended Scope|Notes|
|---|---|---|
|`tools/call:read`|`read-only`|Ideal for analytics and content retrieval servers|
|`tools/call:write`|`write`|State‑changing operations · requires user consent|
|`resources/list`|`metadata`|Safe for directory browsing only|
|`prompt/use`|`interaction`|Optional capability — no state mutations|

Policies should map every tool to a scope and sensitivity level, enforced on both client and server.

---

## 4. Data Protection and Privacy

### 4.1 Encryption and Storage

- **In Transit:** TLS 1.3 ( AES‑256 GCM  or  ChaCha20‑Poly1305 )
- **At Rest:** Full‑disk encryption on server volumes · key rotation every 90 days.
- **Client Secrets:** Environment stores only short‑term sessions (never in source control).

### 4.2 Data Minimization

Follow **“minimum necessary context”** guidelines.  
Agents should never receive more fields than required for the tool’s purpose.   
Mask internal IDs and remove PII before response serialization.

Example masking:

```json
{
  "email": "user****@domain.com",
  "account_id": "acc_#######",
  "result": "query_success"
}
```

### 4.3 Retention and Right to Forget

- Audit logs max 90 days (default).
- Server operators must implement `DELETE /user/:id` endpoint for data removal per GDPR/CCPA.
- Logs retain hash of identifiers (no plain text).

---

## 5. Secure Execution Environment

|Concern|Countermeasure|
|---|---|
|**Code Injection**|Parameterize tool inputs strictly; disable `eval`/shell expanders.|
|**Command Chaining**|Restrict pipe/redirect operators in Unix exec calls.|
|**Resource Exhaustion**|Implement compute and I/O quotas; timeout after 60 s default.|
|**Cross‑Tenant Leakage**|Isolate clients in dedicated containers or namespaces.|
|**Sandbox Escape**|Run tools with AppArmor / SELinux; mount read‑only volumes.|
|**Lateral Movement**|Block non‑approved outbound network requests.|

MCP security testing frameworks should include fuzzing and penetration tests against inputs and transport channels.

---

## 6. Compliance and Governance Frameworks

### 6.1 Applicable Standards

|Standard|MCP Relevance|Description|
|---|---|---|
|**GDPR Art. 25/32**|Data minimization · security by design|Agents must process only necessary fields and protect personal data.|
|**ISO 27001 & SOC 2**|Operational security requirements|Audit every tool invocation and credential rotation.|
|**NIST AI RMF 1.0**|Risk management and HITL oversight|Define acceptable AI risk thresholds per integration.|
|**OWASP LLM Top 10**|Prompt injection  · data exfiltration controls|Sanitize prompt inputs; inspect model responses.|

### 6.2 Governance Controls

|Area|Enforcement|Description|
|---|---|---|
|**Policy Registry**|Enterprise‑level YAML config for approved servers and scopes.|Prevents shadow MCP connections.|
|**Consent Logging**|Host records when user grants tool access.|Supports compliance audits.|
|**Periodic Review**|Quarterly policy and token audit.|Validate scope alignment and de‑scope unused permissions.|

---

## 7. Risk Assessment Checklist

|Risk Category|Detection Method|Mitigation Action|
|---|---|---|
|**Prompt Injection**|Static regex tests and context guards.|Reject LLM inputs containing system commands or URLs.|
|**Data Leakage**|Compare tool outputs to scope allowance.|Apply dynamic redaction middleware before return.|
|**Unrestricted Tools**|Scan tool catalogs for `write` operations.|Review against risk classification.|
|**Expired Certificates**|Monitor TLS expiry alerts < 10 days.|Auto‑rotate certificates via ACME  renewals.|
|**Misconfigured OAuth**|Simulate auth flow attacks.|Harden callback validation and require state params.|

---

## 8. Auditing and Traceability

Every tool invocation should be auditable by both human and machine.

### 8.1 Log Contents

- `timestamp`, `session_id`, `user_id`, `tool_name`, `scope`
- `request_hash` (SHA‑256 of payload)  · `latency_ms`  · `status_code`

### 8.2 Trace Correlations  
Leverage **OpenTelemetry** or **AWS X‑Ray** to bind MCP invocations with external APIs and model events.  
Logs must support multi‑tenant partitioning to prevent data cross‑viewing.

### 8.3 Evaluate Security Metrics

- Mean Time to Detect (MTTD) · Mean Time to Respond (MTTR)
- Unauthorized tool calls per thousand requests
- Credential rotation age and scope properness

---

## 9. Incident Response and Forensics

1. **Detect**  – Trigger alert via SIEM on scope violation or unusual latency.  
2. **Contain**  – Shutdown affected MCP server containers and revoke tokens.  
3. **Eradicate**  – Patch server dependencies and rotate client keys.  
4. **Recover**  – Restore from verified image snapshots; validate protocol compliance.  
5. **Review**  – Generate post‑incident report and update policy.

**Tool:** `mcp-security-audit --report critical --export=json` (official FastMCP CLI extension).

---

## 10. Secure Development Lifecycle (SDL)

Integrate security from design through deployment:

|Stage|Objective|Key Activity|
|---|---|---|
|**Design**|Define risk and scope.|Threat model each tool capability.|
|**Implementation**|Follow secure coding practices.|Static analysis + dependency scans.|
|**Testing**|Verify auth/transport paths.|Pen tests and negative scenario replays.|
|**Deployment**|Lock down configs.|Rotate secrets and validate certificates.|
|**Monitoring**|Detect and alert.|Integrate security telemetry streams.|

---

## 11. Emerging Security Features in MCP (2025 Roadmap)

- **Elicitation Consent Workflows:** Humans approve high‑risk model actions interactively.
- **Per‑Tool Sandbox IDs:** Each tool runs in its own tokenized cgroup namespace.
- **Dynamic Risk Scoring:** MCP clients analyze tool execution risk in real time.
- **Hardware Root of Trust:** Codesigned FIDO2 and TPM attestation for verified servers.

---

## 12. Key Takeaways

1. MCP unlock powerful agentic workflows but expand the attack surface — security must be foundational.  
2. Use **OAuth 2.1 + PKCE**, fine‑grained scopes, and container sandboxing as baseline controls.  
3. Treat server logs as regulated records subject to privacy law.  
4. Adopt a continuous **Security Development Lifecycle (SDLC)** for every tool or connector project.  
5. Compliance is not posture alone — it is the ability to **prove protection and trace every action** within the MCP ecosystem.

---

## Recommended Companion Readings

- [MCP Foundations and Architecture](app://obsidian.md/ai/1_methods-and-systems/MCP/1_mcp-foundations-and-architecture)
- [MCP Runtime and Deployment](app://obsidian.md/ai/1_methods-and-systems/MCP/3_mcp-runtime-and-deployment)
- [Advanced Prompt Engineering for AI and Marketing](app://obsidian.md/ai/1_methods-and-systems/advanced-prompt-engineering)
- [Data Privacy and Compliance (4_ethics-and-governance)](app://obsidian.md/ai/4_ethics-and-governance/data-privacy-and-compliance)
- [Agentic Context Engineering](app://obsidian.md/ai/1_methods-and-systems/prompt-engineering/agentic-context-engineering)

---

> **Summary:**  
> The **Model Context Protocol** creates a powerful and interoperable ecosystem for agents—but security and compliance define its trustworthiness.  
> Implement robust authentication, authorization, sandboxing, and auditing across the stack to ensure that AI integrations operate transparently, ethically, and safely within human‑centered governance frameworks.