# Building Agents with the Claude Agent SDK

## Overview

**The Claude Agent SDK** transforms Large Language Models (LLMs) like Claude into fully capable, tool‑using software agents. It extends a model’s reasoning by granting it access to a **computer‑like environment**—permission to **run code, search files, create or edit content,** and orchestrate multi‑step workflows.

This reference summarizes:

- The core design patterns for developing agents using the Claude Agent SDK
- Key components like the _agent feedback loop_, _subagents_, and _MCP integrations_
- Best‑practice methods for context handling, tool design, and output verification

---

## 1. Purpose of the Claude Agent SDK

The SDK was built to enable agents to work like programmers—or skilled digital assistants—which can:

- **Read, write, and execute code** within a controlled environment
- **Access local and external files or APIs** using Bash and MCP tools
- **Iterate based on verification feedback**, improving reliability
- **Scale horizontally** with subagents handling parallel or domain‑specific tasks

Originally developed to power **Claude Code**, Anthropic’s AI‑assisted coding product, this SDK evolved into a general‑purpose foundation for agentic design across **research, finance, support, and creative** domains.

---

## 2. The Agentic Feedback Loop

Every Claude agent operates through a **closed feedback loop** of reasoning and verification:

```
Gather context → Take action → Verify work → Repeat
```

This process minimizes errors and encourages adaptive self‑correction, making agents more resilient in multi‑step workflows.

|Phase|Objective|Core Functions|
|---|---|---|
|**Gather Context**|Collect relevant data and history.|File search, semantic search, API retrieval.|
|**Take Action**|Execute reasoning and compute tasks.|Run tools, bash scripts, or generate code.|
|**Verify Work**|Evaluate or self‑check outputs.|Use rules, visual feedback, or judge models.|

Agents persist this loop until they achieve a verifiable outcome aligned with task parameters.

---

## 3. Context Management: Giving Claude “a Computer”

Providing Claude access to **system‑level tools** unlocks genuine reasoning autonomy.

|Context Type|Description|Typical Use|
|---|---|---|
|**File System Access**|Directly read, create, edit, and organize files for persistent memory.|Search datasets, manage project files, write reports.|
|**Context Folders**|Structured directories used as knowledge stores (“Conversations,” “Logs”).|Thread‑aware email or ticket assistants.|
|**Agentic Search**|Uses utilities like `grep` and `tail` to scan large files efficiently.|Log analysis or document retrieval.|

Agents use metadata about their environment to **fetch or update context dynamically**, rather than relying solely on static prompts.

### Semantic Search vs. Agentic Search

- _Semantic search_: Embedding-based similarity; faster but less transparent.
- _Agentic search_: Executed with real system commands; slower but accurate.  
    Best practice: begin with agentic search, add semantic only for performance scaling.

---

## 4. Subagents: Parallelization and Context Isolation

**Subagents** are lightweight child processes launched by a parent agent to handle specific tasks.

**Advantages**:

1. **Parallelization** — Run multiple search or analysis tasks concurrently.
2. **Context Isolation** — Each subagent maintains its own memory; only essential results are returned to the main agent.

**Example:**  
An email‑management agent can spawn separate “search subagents” to scan multiple archives simultaneously and merge only relevant message excerpts into a summary.

Subagents are foundational for building **modular, multi‑agent ecosystems**.

---

## 5. Take Action: Tools and Code Execution

Agents act through “tools”—explicit actions registered in the SDK.  
Each tool represents a well‑defined capability that Claude can call independently.

### 5.1 Designing Effective Tools

- Keep tool purpose **clear and atomic** (single responsibility).
- Include strong **input validation** and descriptive documentation strings.
- Use **schema definitions** for predictable I/O structures.

The SDK allows both **custom tool definitions** and generic system commands.

|Example Tool|Description|Sample Use|
|---|---|---|
|`fetchInbox()`|Retrieve recent emails or notifications.|For an assistant agent.|
|`searchFiles()`|Locate matching document content.|For research or legal review agents.|
|`summarizeReport()`|Generate and store summarized results.|For operations or analysis agents.|

See Anthropic’s guide _“Writing Effective Tools for Agents”_ for deep implementation advice.

---

### 5.2 Integrating Bash & Script Capabilities

Agents can run system commands (safely sandboxed) as flexible extensions:

```bash
grep "error" logs/server.log
python process_metrics.py
```

**Used for:**

- Data parsing
- Model test execution
- File transformations

This “Claude with a computer” paradigm allows logical iteration similar to human developers—executing, analyzing, and correcting their own work.

---

### 5.3 Code Generation and Execution

Claude excels at generating and running code as a deterministic way to achieve results.  
Tasks that **translate into executable code** (Python, Bash, SQL) gain reliability because the system can verify success through standard logic—assertions, return codes, or linting.

Common uses:

- Generating structured data (e.g., CSV, Excel, report automation)
- Creating scripts for spreadsheet or document manipulation
- Writing reusable utilities for subagents

Code generation increases precision, making the agent’s behavior auditable and repeatable.

---

### 5.4 Model Context Protocol (MCP) Integrations

**MCP** provides standardized connectors to external systems—handling **authentication, API calls, and data exchange automatically.**

|Example Integration|Agent Action|
|---|---|
|Slack MCP|“search_messages” for project mentions|
|GitHub MCP|“read_issues” or “update_pull_request”|
|Asana MCP|“create_task” or “check_status”|
|Google Drive MCP|“upload_file” or “fetch_docs”|

Instead of writing API plumbing, developers register these endpoints once and call them as recognized tools.

---

## 6. Verification: Building Reliable Self‑Checking Agents

Verification closes the feedback loop, helping agents detect and fix errors independently.

### 6.1 Rule‑Based Evaluation

Define validation logic (similar to **linting**) that checks each output against structural or semantic rules.  
Example rules:

- “Email has valid address format”
- “Response JSON matches schema”

When a rule fails, the model receives contextual error feedback for re‑attempt.

### 6.2 Visual Feedback

For visually structured outputs (HTML, dashboards, UIs), agents can use screenshots or render previews for visual comparison.  
Using an MCP server like **Playwright**, an agent can:

- Render HTML/email layouts
- Capture screenshots in various viewport sizes
- Re‑evaluate presentation consistency

Visual verification bridges reasoning and interface design tasks.

### 6.3 LLM‑as‑Judge

Agents can delegate quality checks to a secondary model ("judge agent") that reviews tone, style, or reasoning integrity.  
Though slower, this enhances subjective evaluation like tone fit or clarity.

> 🧩 **Tip:** Combine rule‑based and LLM‑as‑judge methods—rules for precision, models for qualitative review.

---

## 7. Continuous Testing and Improvement

Reliable agents evolve through **iterative evaluation** (“evals”).  
Implement structured monitoring metrics such as:

- **Task success rate**
- **Average self‑correction cycles**
- **Tool utilization accuracy**
- **Latency between perception and completion**

### Example Evaluation Checklist

|Question|Purpose|
|---|---|
|Does the agent have the correct context schema?|Prevent missing state data.|
|Do tools match its task set?|Avoid misfires or redundant actions.|
|Are rule failures explained clearly to the model?|Enable effective self‑correction.|
|Are subagents underutilized or overused?|Optimize memory token usage.|

Building lightweight eval pipelines (similar to OpenAI’s trace grading) ensures that Claude agents improve over time without manual re‑prompting.

---

## 8. Example Use Cases

|Category|Description|Key SDK Features|
|---|---|---|
|**Finance Agent**|Evaluates portfolios, fetches API data, runs calculations.|Bash tools + MCP connectors.|
|**Personal Assistant Agent**|Manages schedule, books travel, handles email.|Subagents + file system memory.|
|**Customer Support Agent**|Triages tickets, retrieves account info, escalates cases.|MCP integration + semantic search.|
|**Research Agent**|Performs literature reviews and data analysis.|Code execution + context compaction.|
|**Creative Production Agent**|Writes scripts, generates images, validates layouts.|Code generation + visual feedback.|

Each agent follows the same **core feedback loop**, customized by toolset and context.

---

## 9. Best Practices

|Domain|Recommendation|
|---|---|
|**Tools Design**|Define minimal, composable tool actions. Avoid overlapping responsibilities.|
|**Context Compaction**|Enable automatic summarization when context window approaches limits.|
|**Parallelization**|Use subagents for long or repetitive subtasks to conserve context tokens.|
|**Prompt Hygiene**|Document agent purpose and safety constraints clearly within system instructions.|
|**Testing & Governance**|Run eval scripts regularly; log tool calls, failures, and self‑fix attempts.|
|**Security**|Sandbox code execution and restrict system or network access; monitor local writes.|

Embedding these ensures production‑grade consistency and compliance.

---

## 10. Getting Started

1. **Install the SDK**
    
    ```bash
    pip install claude-agent-sdk
    ```
    
    or follow [Anthropic’s documentation](https://docs.claude.com/en/api/agent-sdk/overview).
    
2. **Define Tools and Memory**  
    Register primary callable actions and persistent storage.
    
3. **Implement the Feedback Loop**  
    Program gathering, acting, and verifying stages using the SDK’s orchestration API.
    
4. **Add MCP Integrations**  
    Connect to Slack, GitHub, or other systems without manual API plumbing.
    
5. **Iterate and Evaluate**  
    Use rule‑based tests and log summaries to monitor accuracy improvements.
    

---

## Key Takeaways

1. The **Claude Agent SDK** operationalizes the “agent feedback loop,” enabling LLMs to act, verify, and improve autonomously.
2. By **granting system‑level access** (files, tools, Bash), agents can perform realistic, multi-step workflows beyond chat.
3. **Subagents and MCP integration** make scalable, parallelized, and integrated designs feasible.
4. **Verification layers** (rules, visual, or judge agents) ensure outputs remain accurate and safe.
5. Robust context management and continual testing transform generative systems into **autonomous, reliable digital collaborators**.

---

## Recommended Resources

- [AI Agents Running Workflows](app://obsidian.md/ai/1_methods-and-systems/agents/ai-agents-running-workflows)
- [How to Build Full‑Stack Agent Apps](app://obsidian.md/ai/1_methods-and-systems/agents/how-to-build-fullstack-agent-apps)
- [Introduction to OpenAI Agent Builder](app://obsidian.md/ai/1_methods-and-systems/agents/introduction-to-openai-agent-builder)
- [Agentic Context Engineering](app://obsidian.md/ai/1_methods-and-systems/prompt-engineering/agentic-context-engineering)
- [Advanced Prompt Engineering for AI and Marketing](app://obsidian.md/ai/1_methods-and-systems/prompt-engineering/advanced-prompt-engineering)

---

> **Summary:**  
> The Claude Agent SDK represents a milestone in agentic architecture—bringing together reasoning, code execution, and self‑validation within a unified system.  
> By mastering context control, tool design, and verification feedback, developers can build **trustworthy, autonomous agents** capable of real digital work across disciplines.