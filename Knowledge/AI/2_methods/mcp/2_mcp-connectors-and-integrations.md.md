# MCP Connectors and Integrations

## Building Bridges Between AI Agents and External Systems

---

## 1. What Are MCP Connectors?

**Model Context Protocol (MCP)** connectors are the structured interfaces that link AI agents to outside applications and data.  
A connector tells an AI model:

> “Here’s how you can talk to my service, what tools you can call, and what you’ll get back.”

Every MCP server exposes its connectors as a catalog of **tools**, **resources**, and **prompts**, each with clearly defined schemas for discovery and safe execution.

Connectors can be built for local utilities (files, databases, IDEs) or remote platforms (Supabase, GitHub, Google Ads, WordPress, Chrome DevTools etc.). They let agents access live data, execute actions, and integrate with enterprise systems — without the agent needing direct API logic or credentials.

---

## 2. Connector Architecture and Roles

|Layer|Purpose|Examples|
|---|---|---|
|**Server Layer**|Implements MCP spec endpoints and defines tools/resources/prompts.|`supabase-mcp`, `chrome-devtools-mcp`, `ai-engine-mcp`|
|**Connector Descriptor (JSON)**|Declares entry‑point (`command`, `args`, `env`) so the client knows how to invoke.|`mcp.json`, `~/.client/mcp_servers.json`|
|**Transport**|How messages flow — stdio (for local CLI or editor) or HTTP (remote cloud servers).|`npx @supabase/mcp …` / `https://mcp.supabase.com/mcp`|
|**Host Integration**|How the user’s AI environment registers and uses the connector.|Claude Desktop config, Cursor IDE settings, JetBrains Junie panel|

Each connector communicates through **JSON‑RPC 2.0 requests** (`tools/call`, `resources/read`, `prompts/list`) and structured JSON schemas that define inputs and outputs, ensuring cross‑client compatibility.

---

## 3. Types of Connectors

|Category|Description|Typical Target|
|---|---|---|
|**Database Connectors**|Expose data queries and CRUD actions safely to LLMs.|Postgres (Supabase), MongoDB, MySQL|
|**Application/Platform Connectors**|Wrap web APIs for AI use — project management, marketing, social.|GitHub, Google Ads, Slack, WordPress|
|**Developer Tool Connectors**|Integrate IDE or dev‑ops functions.|Chrome DevTools, Cursor, Junie for PhpStorm|
|**Automation and Workflow Connectors**|Trigger scripts and services in CI/CD, storage, or business apps.|Activepieces, Zapier, Airflow|
|**Local Utility Connectors**|Provide OS tools, file access, docs, or search without cloud calls.|mcp‑obsidian, SearXNG search server|

---

## 4. Common Integration Patterns

### 4.1 Local (Development or Offline)

- **Transport:** `stdio` via `npx`, `uvx`, or CLI command.
- **Use Case:** Agents running inside local IDEs (e.g., Cursor, LM Studio).
- **Pros:** Full privacy, direct file/system access.
- **Example:** SearXNG MCP (server for private web searches).

### 4.2 Remote (HTTP / Streamable HTTP)

- **Transport:** `https://…/mcp` endpoint with Streamable HTTP responses.
- **Use Case:** Cloud services and enterprise APIs.
- **Pros:** Broad client compatibility (ChatGPT, Claude, Gemini).
- **Example:** Supabase MCP Server (`https://mcp.supabase.com/mcp`).

### 4.3 Hybrid / Federated Connectors

Complex agents combine several servers: e.g., a browser MCP for data gathering plus a database MCP for analysis, coordinated by a workflow agent.

---

## 5. Leading Example Connectors

|Connector|Purpose|Key Integration Notes|
|---|---|---|
|**Supabase MCP**|Remote database and auth integration for agents (e.g., Claude Code, ChatGPT).|Supports OAuth 2 auth, Remote HTTP; feature groups (database, storage, functions).|
|**Chrome DevTools MCP**|Gives AI direct browser debugging and performance analysis capabilities.|Uses Puppeteer and DevTools protocol; ideal for developer agents.|
|**AI Engine (WordPress MCP)**|Connects Claude or ChatGPT to WordPress via AI Engine plugin.|Supports 30+ WordPress operations (create posts, upload media, modify themes).|
|**Google Ads API MCP**|Read‑only ad metrics access via secure remote server.|Early open‑source release; adds analytics tools to marketing agents.|
|**Context7 Memory MCP**|Persistent agent memory and RAG context service.|Supports multi‑agent context sharing and vector store query.|
|**Laravel Boost MCP**|For PhpStorm Junie coding agent to interface with Laravel framework.|Lists Artisan commands, logs, routes, config files for debugging.|

---

## 6. Connector Configuration in Clients

### Example for Claude Desktop / Cursor IDE

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["chrome-devtools-mcp@latest"]
    },
    "supabase": {
      "url": "https://mcp.supabase.com/mcp",
      "auth": "OAuth2"
    }
  }
}
```

### JetBrains Junie Configuration (File: `~/.junie/mcp.json`)

```json
{
  "servers": [
    {
      "name": "laravel-boost",
      "command": "php",
      "args": ["vendor/bin/laravel-mcp"]
    }
  ]
}
```

### WordPress AI Engine Bridge

Node relay example (`mcp.js`):

```bash
ai-engine/labs/mcp.js add https://example.com TOKEN
ai-engine/labs/mcp.js start example.com
```

This patches Claude’s local config to connect directly to WordPress via AI Engine.

---

## 7. SDKs and Tools for Connector Development

|SDK|Language|Description|
|---|---|---|
|**FastMCP**|Python|Lightweight server framework for 快速 prototyping MCP servers (e.g., data science and document access agents).|
|**MCP PHP SDK**|PHP|Official collaboration between PHP Foundation and Symfony; framework‑agnostic.|
|**ModelContextProtocol JS SDK**|TypeScript|Used to build npm‑published servers (e.g., Chrome DevTools MCP).|
|**Supabase Remote Server Template**|JavaScript|API for building read‑write database and storage MCPs.|

Each SDK abstracts the JSON‑RPC boilerplate and provides helpers for tool declaration, schema validation, and Server lifecycle management.

---

## 8. Connector Discovery and Security

### 8.1 Discovery Protocol (`tools/list`, `resources/list`)  
Clients dynamically query servers to see what capabilities are available. Every tool is defined with:

- `name`, `description`, `input_schema`, `output_schema`
- Access scopes and version

### 8.2 Authentication & AuthZ

- **Local Servers:** No network; trust boundary = system user.
- **Remote Servers:** OAuth 2.1 + scopes mandatory (March 2025 spec update).
- **Fine‑Grained Permissions:** Servers declare allowable read/write ops per tool; clients prompt for consent.

### 8.3 Audit and Logging  
Hosts record full JSON‑RPC exchange IDs and timestamps. Enterprises typically pipe logs to SIEM or AI Governance dashboards.

---

## 9. Best Practices for Integration Design

1. **Scope Matters:** Limit exposed tools to one domain (e.g., analytics, CMS).  
2. **Use Versioning:** Break changes require tool or server version bump.  
3. **Provide Clear Schemas and Docs:** Agents parse better with explicit types.  
4. **Implement Graceful Failure:** Return structured errors with codes.  
5. **Separate Read and Write:** Mark tools as `readonly` when appropriate for safety.  
6. **Support Elicitation:** Use human confirmation steps for state‑changing operations.  
7. **Test Against Multiple Clients:** Cursor (FastMCP), Claude (Desktop), Junie.  
8. **Respect Timeouts and Streaming Limits:** Avoid stranded sessions.

---

## 10. Ecosystem Trends in Connectors (2024–2025)

- **Growth and Concentration:** Top 10 MCP servers capture ~45 % of public usage (GitHub stars metric).  
- **Dominant Use Cases:** Software Engineering (24.7 %), Web Automation (24.8 %), Database & Search (23.1 %).  
- **Hybrid Access Patterns:** Most servers offer both _read_ and _write_ operations; few are read‑only.  
- **Enterprise Focus:** Vendors are adding fine‑grain scope management, local processing, and multi‑API fallbacks.

(See _MCP in Practice_ by O’Reilly 2025 for dataset analysis of 2,874 servers.)

---

## 11. Testing and Validation for Connectors

When building MCP integrations, validate at three levels:  
1. **Specification Compliance** — respond correctly to `tools/list`, `tools/call` and report schema conformity.  
2. **Security Conformance** — verify token scopes and deny unauthorized calls.  
3. **Functional Correctness** — mock tool calls and capture structured outputs for QA.

Supabase and Anthropic provide open‑source _MCP Inspector_ utilities for automated testing.

---

## 12. Key Takeaways

1. **Connectors are how AI agents touch the real world.** They translate AI intent into structured interactions.  
2. **MCP standardizes integration discovery and execution** across vendors.  
3. Practical adoption spans hours—from local privacy‑focused setups to enterprise cloud services.  
4. **Security, versioning, and schema design determine trust.** Always audit inputs, outputs, and access scopes.  
5. **SDKs like FastMCP and MCP PHP SDK** make custom connector creation accessible to engineering teams.  
6. Well‑designed connectors enable **elastic agent workflows**—mixing database access, tool automation, and real‑time context enrichment.

---

## Recommended Companion Materials

- [MCP Foundations and Architecture](app://obsidian.md/ai/1_methods-and-systems/MCP/1_mcp-foundations-and-architecture)
- [How MCP Is Making AI Agents Actually Do Things in the Real World](app://obsidian.md/ai/1_methods-and-systems/MCP/how-mcp-is-making-ai-agents-actually-do-things-in-the-real-world)
- [Building MCP Servers: 15 Best Practices](app://obsidian.md/ai/1_methods-and-systems/MCP/building-mcp-servers)
- [MCP‑PHP SDK Overview](app://obsidian.md/ai/1_methods-and-systems/MCP/mcp-php-sdk)
- [Model Context Protocol vs Function Calling vs OpenAPI Tools](app://obsidian.md/ai/1_methods-and-systems/MCP/model-context-protocol-vs-function-calling-vs-openapi-tools)

---

> **Summary:**  
> MCP connectors are the operational interfaces that make agentic AI practical. They turn model requests into structured, governed actions on real systems through open standards and SDKs. From Supabase to Chrome DevTools to local search and memory servers, these connectors form the web of interactivity that defines the modern agentic AI ecosystem.