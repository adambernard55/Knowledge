# Building Full‑Stack Agent Applications with Gemini, CopilotKit, and LangGraph

## Overview

Modern **agentic AI applications** go beyond chat interfaces — they combine **frontend interactivity**, **stateful backend workflows**, and **LLM‑based reasoning** across an integrated architecture.  
This reference outlines how to build full‑stack AI agent apps using:

- **Google Gemini** — the underlying large language model (LLM).
- **CopilotKit SDK** — for connecting front‑end components and agent workflows.
- **LangGraph** — to design multi‑step, stateful agent logic and tool execution.
- **FastAPI** and **Next.js** — providing backend orchestration and frontend interaction.

The example case includes two practical agents:

1. **Post Generator Agent** — generates LinkedIn and X (Twitter) posts based on live search results.
2. **Stack Analyzer Agent** — evaluates a public GitHub repository to infer its technology stack.

---

## 1. What You’ll Build

|Agent|Description|Key Capabilities|
|---|---|---|
|**Post Generator**|Creates LinkedIn/X posts grounded in live web search.|Multi‑step workflow integrating Gemini + Google Search tools.|
|**Stack Analyzer**|Analyzes a GitHub repo and returns a structured tech‑stack summary.|Uses Gemini structured output with LangGraph validation.|

Both applications showcase a **front‑to‑back integration loop**:  
**UI → CopilotKit Runtime → FastAPI backend → LangGraph workflows → Gemini reasoning → streamed results back to UI.**

---

## 2. Core Tech Stack and Architecture

**Frameworks and Components**

|Layer|Technology|Function|
|---|---|---|
|**Frontend**|[Next.js 15 (TypeScript)](https://nextjs.org/)|UI framework and API routes|
|**Agent SDK**|[CopilotKit](https://copilotkit.ai/)|Connects UI with backend agents|
|**Backend**|[FastAPI](https://fastapi.tiangolo.com/)|Serves REST/GraphQL endpoints|
|**Workflow Engine**|[LangGraph](https://langchain.com/langgraph)|Builds stateful, node‑based workflows|
|**Model**|[Google Gemini](https://gemini.google.com/)|LLM for reasoning and generation|
|**Schema Validation**|[Pydantic](https://docs.pydantic.dev/latest/)|Ensures structured JSON outputs|
|**Runtime Server**|[Uvicorn](https://www.uvicorn.org/)|ASGI server for FastAPI|

**High‑Level Data Flow**

```
Browser UI (CopilotChat)
     ↓
Next.js API route  →  CopilotKit Runtime (GraphQL)
     ↓
FastAPI backend  →  LangGraph workflow (Python)
     ↓
Gemini model & tools  →  Streaming responses
     ↓
Front‑end renderer (chat, logs, cards)
```

This architecture enables **real‑time feedback**, structured JSON streaming, and extensible tool integration.

---

## 3. Project Setup and Structure

Project directory structure:

```
.
├── frontend/             ← Next.js frontend
│   ├── app/
│   │   ├── post-generator/
│   │   ├── stack-analyzer/
│   │   ├── api/copilotkit/
│   │   └── contexts/
│   └── components/
├── agent/                ← FastAPI backend
│   ├── main.py
│   ├── posts_generator_agent.py
│   ├── stack_agent.py
│   ├── prompts.py
│   └── agent.py
└── README.md
```

Each environment (frontend and backend) maintains its own `.env` file to store API keys and variables, such as:

```
GOOGLE_API_KEY=<your-gemini-key>
```

Use [Poetry](https://python-poetry.org/) for Python dependency management and `pnpm` or `npm` for frontend packages.

---

## 4. Frontend Setup (Next.js 15 + CopilotKit)

### 4.1 Install Core Packages

```bash
pnpm install copilotkit @copilotkit/react-core @copilotkit/react-ui @copilotkit/runtime @copilotkit/runtime-client-gql
```

These packages provide:

- **Core runtime connectivity** (`@copilotkit/react-core`)
- **Ready‑made chat UI** (`@copilotkit/react-ui`)
- **Backend and client runtime bindings** (`@copilotkit/runtime`)

### 4.2 Global Layout and Providers

Wrap the entire application in `CopilotKit` to share context and active agent state:

```tsx
<CopilotKit runtimeUrl="/api/copilotkit" agent={layoutState.agent}>
  {children}
</CopilotKit>
```

A custom `LayoutContext` tracks which agent (post_generation or stack_analysis) is active based on the route, ensuring that UI requests trigger the correct backend workflow.

### 4.3 API Route Proxying

Next.js API routes forward CopilotKit events to the backend FastAPI endpoint, abstracting away CORS or environment differences.  
This proxy handles GraphQL‑like agent communications with Gemini via the Copilot Runtime.

### 4.4 Agent Interfaces

UI pages — `post-generator/page.tsx` and `stack-analyzer/page.tsx` — use CopilotChat to build interactive interfaces.

- `useCopilotChatSuggestions` populates prompt suggestions.
- `useCopilotAction` defines actions and output renderers (e.g., LinkedIn/X post cards).
- `useCoAgentStateRender` visualizes streaming tool logs or intermediate results.

The frontend thus remains declarative, while all logic resides in modular backend graphs.

---

## 5. Backend Setup (FastAPI + LangGraph + CopilotKitSDK)

### 5.1 Python Environment

```bash
poetry add fastapi uvicorn copilotkit langgraph langchain langchain-google-genai google-genai pydantic python-dotenv
```

### 5.2 FastAPI Service Entrypoint

`main.py` registers two agents (Post Generation and Stack Analysis) and mounts a unified `/copilotkit` endpoint:

```python
from fastapi import FastAPI
from copilotkit import CopilotKitSDK, LangGraphAgent
from copilotkit.integrations.fastapi import add_fastapi_endpoint
from posts_generator_agent import post_generation_graph
from stack_agent import stack_analysis_graph

app = FastAPI()
sdk = CopilotKitSDK(agents=[
    LangGraphAgent(name="post_generation_agent", graph=post_generation_graph),
    LangGraphAgent(name="stack_analysis_agent", graph=stack_analysis_graph),
])
add_fastapi_endpoint(app, sdk, "/copilotkit")
```

This SDK handles streaming state updates back to the React client automatically.

---

## 6. Agent Logic (LangGraph Workflows)

### 6.1 LangGraph Overview

LangGraph models each agent as a **state machine** built from nodes.  
Each node performs a discrete task such as reasoning, analysis, or tool invocation.  
Developers wire these nodes into `StateGraph` objects and compile them with `MemorySaver` checkpointing.

### 6.2 Post Generator Agent

Implements a simple three‑node graph:

1. **chat_node** — interprets user intent and performs web search using Gemini’s `google_search` tool.
2. **fe_actions_node** — generates final linked posts (LinkedIn + X) based on retrieved information.
3. **end_node** — cleans up and returns structured output.

### 6.3 Stack Analyzer Agent

Parses and analyzes GitHub repository data via nodes:

1. **gather_context_node** — pulls repository metadata, readme, and manifest data from the GitHub API.
2. **analyze_with_gemini_node** — constructs an analysis prompt and invokes Gemini for reasoning, constrained by a `StructuredStackAnalysis` Pydantic schema.
3. **end_node** — returns validated, structured JSON for easy frontend visualization.

By expressing agent logic as node flows, LangGraph ensures **deterministic orchestration, resilience**, and **state persistence**.

---

## 7. Prompt Design and Structured Outputs

### 7.1 System Prompts

Centralized in `prompts.py` to define model persona and behavioral instructions:

- `system_prompt`: Guides Gemini to use web search when generating posts.
- `system_prompt_3`: Defines style and tone rules for social‑media content.

### 7.2 Structured Output Tools

Use Pydantic models and LangChain tool wrappers to enforce output consistency:

```python
@tool("return_stack_analysis", args_schema=StructuredStackAnalysis)
def return_stack_analysis_tool(**kwargs):
    validated = StructuredStackAnalysis(**kwargs)
    return validated.model_dump(exclude_none=True)
```

This guarantees JSON schema integrity across runs and allows the frontend to render analysis cards predictably.

---

## 8. End‑to‑End Flow Summary

|Stage|Description|
|---|---|
|**1. User Input**|The user prompts the interface (CopilotChat).|
|**2. Next.js Proxy**|The message is forwarded to `/api/copilotkit` (Copilot Runtime).|
|**3. Backend Router**|FastAPI routes request to a LangGraph agent via CopilotKitSDK.|
|**4. Workflow Execution**|The StateGraph executes nodes, invoking Gemini and tools.|
|**5. Streaming Updates**|CopilotKit streams messages, logs, and tool events to the UI.|
|**6. Rendered Output**|The UI displays chat, logs, and structured results (posts or repo cards).|

---

## 9. Running the Project

### 9.1 Start Backend

```bash
cd agent
poetry install
poetry run python main.py
```

### 9.2 Start Frontend

```bash
cd frontend
pnpm install
pnpm run dev
```

Visit `http://localhost:3000/post-generator` to use the Post Generator, or navigate to `/stack-analyzer` for repository analysis.

---

## 10. Best‑Practice Design Principles

|Aspect|Recommendation|
|---|---|
|**State Management**|Keep workflows modular; each agent owns its own state machine.|
|**Security**|Never expose API keys to the frontend; use environment variables only on backend.|
|**Reusability**|Place prompts and helper tools in shared modules for cross‑agent use.|
|**Observability**|Stream `tool_logs` and debug states in the UI for rapid testing.|
|**Scalability**|Decouple agent logic from service implementations; support multiple registered agents.|
|**Compliance**|Review API usage limits and ensure responsible LLM call patterns.|

---

## 11. Common Use Cases for Full‑Stack Agent Apps

- **Knowledge copilots:** Internal assistants that search across enterprise documents.
- **Code or design copilots:** Analyze repos, build documentation scaffolds.
- **Marketing generators:** Automate campaign content with tone control and QA workflows.
- **Task orchestration dashboards:** Multi‑agent collaboration hubs (e.g., summarization + analysis).

This architecture serves as a **reference implementation** for developing any extensible, web‑based agentic AI tool.

---

## Key Takeaways

1. **Full‑stack agents require coordination between frontend UI, runtime, and backend workflows.**
2. **CopilotKit** provides an elegant way to connect front‑end interactions with AI agent APIs.
3. **LangGraph** manages complex stateful behaviors and enables persistent, modular agent design.
4. **Gemini’s multimodal and tool‑use features** make it ideal for grounded content generation and structured analysis.
5. **Layered design ensures flexibility**—the same pattern works for agents using OpenAI, Claude, or other LLMs.
6. The result: a reliable reference for creating **production‑ready, interactive AI agent applications.**

---

## Further Reading

- [Agentic AI Overview](app://obsidian.md/ai/1_methods-and-systems/agentic-ai-overview)
- [Building Agents with the Claude Agent SDK](app://obsidian.md/ai/1_methods-and-systems/agents/building-agents-with-the-claude-agent-sdk)
- [Introduction to OpenAI Agent Builder](app://obsidian.md/ai/1_methods-and-systems/agents/introduction-to-openai-agent-builder)
- [Agentic Context Engineering](app://obsidian.md/ai/1_methods-and-systems/prompt-engineering/agentic-context-engineering)
- [Advanced Prompt Engineering for AI](app://obsidian.md/ai/1_methods-and-systems/prompt-engineering/advanced-prompt-engineering)

---

> **Summary:**  
> Building full‑stack agent applications merges the power of front‑end interactivity, back‑end workflow orchestration, and large‑language‑model reasoning.  
> By combining **Gemini**, **CopilotKit**, and **LangGraph**, developers can implement robust, streaming, and stateful agent systems to automate complex workflows — a foundational capability for next‑generation agentic AI platforms.