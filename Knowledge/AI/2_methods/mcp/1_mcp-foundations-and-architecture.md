# MCP Foundations and Architecture

## Understanding the Model Context Protocol

---

## 1. Overview

The **Model Context Protocol (MCP)** is an **open standard** that defines how AI systems—particularly agentic large language models (LLMs)—connect to real-world data and tools.

Instead of having every assistant, IDE, or chatbot build its own custom integrations, MCP acts as a **universal bridge** between AI reasoning engines and the applications or APIs they depend on.

MCP was introduced by **Anthropic in 2024** and rapidly adopted by organizations such as **OpenAI, Microsoft, Supabase, JetBrains, and Google**, making it a foundational layer of the **agentic AI ecosystem**.

It is often referred to as _“the USB‑C of AI”_: one consistent plug that connects models to external systems safely and predictably.

---

## 2. Core Architectural Model

MCP defines a **three‑component architecture** built around clarity, portability, and separation of responsibilities:

|Role|Function|Example|
|---|---|---|
|**Host**|The environment where users interact with the AI application. It manages sessions, user input, and result display.|_Claude Desktop, ChatGPT, Cursor, Junie (JetBrains)_|
|**Client**|The logic component that translates model intentions into MCP protocol actions, managing the message exchange loop.|_MCP client built into Claude, Cursor, or VS Code Copilot_|
|**Server**|A wrapper around real‑world resources—databases, APIs, files—that exposes a structured, discoverable interface to clients.|_Supabase MCP server, Chrome DevTools MCP, WordPress AI Engine MCP_|

The communication between **client** and **server** follows a defined JSON‑RPC pattern over approved transports (stdio or HTTP), ensuring interoperability across tools and ecosystems.

---

## 3. Protocol Layers

### 3.1 Transport Layer

Handles how data moves between clients and servers.

- **Local (stdio)** – Best for command‑line tools, IDEs, and local development. Simple, secure, and connection-rich.
- **Remote (HTTP)** – Suitable for cloud applications; supports _Streamable HTTP_ for large or real‑time responses.
- **Deprecation note:** SSE (Server‑Sent Events) is being replaced by Streamable HTTP in 2025.

### 3.2 Protocol Layer

Defines the message schema using **JSON‑RPC 2.0**, a minimal and human‑readable standard for remote procedure calls:

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {...},
  "id": "1234"
}
```

Every request and response is explicitly typed, versioned, and traceable for debugging and validation.

### 3.3 Capability Layer

The layer that gives MCP its power — the **contracts** of what a server can do.  
MCP defines three primary primitive types:

|Primitive|Description|Example|
|---|---|---|
|**Tools**|Executable functions the agent can call (actions).|`execute_sql_query`, `create_post`, `navigate_page`|
|**Resources**|Read‑only objects or data streams the agent can access.|`file:///reports/q3.pdf`, `db://sales/stats`|
|**Prompts**|Pre‑structured templates that help guide reasoning or parameterized workflows.|“Summarize document”, “Compare top product metrics”|

Together, they define a _language‑agnostic API surface_ that any model can discover dynamically.

---

## 4. Workflow: How MCP Works in Practice

### Step‑by‑Step Flow

```
User → Host → Client → Server → External System → Return Path → Host
```

1. **Discovery** — The client locates available MCP servers (via config or registry).
2. **Capability negotiation** — The client queries each server (`tools/list`) to learn what actions/resources it exposes.
3. **Request creation** — The AI model formulates a call (e.g., “get project stats”) that maps to a specific tool.
4. **Transport** — The request travels via JSON‑RPC over stdio or HTTP.
5. **Execution** — The server translates the request into underlying system actions and returns a normalized response.
6. **Aggregation** — The client merges results from multiple servers if needed.
7. **Presentation** — The host displays the refined result to the user.

This unified loop lets models operate safely on live systems without direct, ad‑hoc API integrations.

---

## 5. Design Philosophy

MCP’s design embeds several **guiding principles** from modern software architecture:

|Principle|Meaning|Benefit|
|---|---|---|
|**Composable Interfaces**|Build once, reuse anywhere.|Avoid re‑writing integrations.|
|**Security by Boundary**|Servers enforce capabilities and scopes.|Prevents uncontrolled model actions.|
|**Protocol Transparency**|JSON‑based logs and schemas are human‑readable.|Easier audits, testing, and debugging.|
|**Extensibility**|Add new primitives or transports without breaking old systems.|Future‑proof compatibility across vendors.|
|**Human‑in‑the‑Loop Readability**|Requests and results are understandable by developers.|Simplifies verification and compliance review.|

---

## 6. Architectural Components in Detail

### 6.1 Host

- Provides the user interface and session memory.
- Manages user permissions and session lifecycle.
- Exposes client connections (desktop, IDE, cloud app).

### 6.2 Client

- Implements discovery (`mcpServers` registry) and negotiation.
- Handles the JSON‑RPC loop and maintains request IDs.
- Translates LLM intentions into typed server calls.

### 6.3 Server

- Defines tool catalog, schemas, and prompt templates.
- Handles authorization, throttling, and data access logic.
- Returns machine‑and‑human readable results with logs.

### 6.4 Example Interaction Diagram

```
+---------+        +----------+        +----------+        +-------------+
|  User   | -----> |  Host    | -----> |  Client  | -----> |   Server    |
|         |        | (Claude) |        | (Agent)  |        | (Supabase)  |
+---------+        +----------+        +----------+        +-------------+
       | display         | session mgmt      | JSON-RPC          | SQL / API calls
       +-----------------------------------------------------------------------→ External System
```

---

## 7. Key Advantages

|Benefit Area|Description|
|---|---|
|**Standardization**|One consistent way for AI to access tools across ecosystems.|
|**Security**|Controlled server boundaries prevent data leakage.|
|**Governance**|Logs and schemas create full audit trails.|
|**Extensibility**|MCP Servers for databases, browsers, CRM, IDEs, or custom workflows.|
|**Portability**|Works across vendors (Anthropic, OpenAI, JetBrains, Microsoft).|
|**Reusability**|Each server serves many clients with no duplication.|

---

## 8. MCP in the Broader Agent Stack

MCP sits in the **integration layer** of the emerging AI stack:

```
Applications (ChatGPT, Claude, VS Code AI)
        │
Agent & Orchestration (LangGraph, AgentKit, Claude SDK)
        │
Integrations Layer (MCP servers + MCP clients)
        │
Data & System APIs (Databases, SaaS APIs, Local Tools)
```

By abstracting tool usage, MCP allows agentic frameworks to plug into enterprise data, cloud APIs, or local utilities transparently—not unlike how REST standardized API interaction two decades ago.

---

## 9. Real‑World Implementations

|MCP Implementation|Description|Platform|
|---|---|---|
|**Supabase MCP**|Exposes database, storage, and advisor APIs; supports both stdio & Streamable HTTP.|Developer & data pipeline integration|
|**Chrome DevTools MCP**|Provides browser inspection, navigation, and performance tools for coding agents.|Web development|
|**WordPress AI Engine MCP**|Enables Claude to create, publish, and manage posts securely.|CMS automation|
|**MCP‑PHP‑SDK**|Official SDK for building servers in PHP.|IDE & backend integration|
|**Context7 Memory MCP**|Handles long‑term context and recall for agentic workflows.|Persistent memory layer|

Each demonstrates how MCP can extend different domains without changing the base architecture.

---

## 10. Comparison: MCP vs Legacy Integration Methods

|Feature|**MCP**|**Traditional API / Plugin**|
|---|---|---|
|**Discovery**|Standardized (`tools/list`)|Manual integration|
|**Communication**|JSON‑RPC protocol|Custom HTTP or SDK calls|
|**Security**|Scoped permissions per tool|Often app‑level|
|**Portability**|Multi‑client/server interoperability|App‑specific|
|**Extensibility**|Add new transports|Recompile entire system|
|**Human Review**|Transparent logs|Often opaque logic|

---

## 11. Current Challenges and Evolving Standards

- **Concentration Risk:** Market power clustering around a few major servers (GitHub, Browser Use, Supabase).
- **Security & Trust:** Open, unaudited third‑party servers introduce verification risks.
- **Performance Variability:** Different transport implementations yield inconsistent latency.
- **Standard Evolution:** 2025 updates add Streamable HTTP and Elicitation (Human‑in‑the‑Loop) features—still unevenly supported.

Active community discussions focus on defining **versioned discovery**, **sandboxing requirements**, and **dynamic consent interfaces**, particularly for enterprise deployment.

---

## 12. Conclusion

The **Model Context Protocol** forms the backbone of the agentic AI ecosystem.  
It turns language models from _isolated reasoning systems_ into _interactive, context‑aware digital workers_ capable of performing structured tasks.

By establishing clear roles (host, client, server) and using a portable JSON‑RPC architecture, MCP scales from small local automations to enterprise‑grade integrations.  
Just as HTTP standardized the web, **MCP is standardizing tool access for AI**—enabling interoperability, transparency, and human oversight in real‑world automated reasoning.

---

## Recommended Reading

- [AI Agents Running Workflows](app://obsidian.md/ai/1_methods-and-systems/agents/ai-agents-running-workflows)
- [Building Agents with the Claude Agent SDK](app://obsidian.md/ai/1_methods-and-systems/agents/building-agents-with-the-claude-agent-sdk)
- [Introduction to OpenAI Agent Builder](app://obsidian.md/ai/1_methods-and-systems/agents/introduction-to-openai-agent-builder)
- [Agentic Context Engineering](app://obsidian.md/ai/1_methods-and-systems/prompt-engineering/agentic-context-engineering)

---

> **Summary:**  
> The **Model Context Protocol (MCP)** defines the standard interface between AI agents and external data, systems, and tools, creating a secure and extensible foundation for agentic computing.  
> By clarifying architecture and standardizing interoperability, MCP transforms large language models from passive content generators into **connected, action‑capable artificial intelligences.**