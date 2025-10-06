# Here's How To Build Fullstack Agent Apps (Gemini, CopilotKit & LangGraph)

[#programming](https://dev.to/t/programming)[#webdev](https://dev.to/t/webdev)[#opensource](https://dev.to/t/opensource)[#ai](https://dev.to/t/ai)

AI agents are getting close to real world applications, but most developers still find it complex to build one.

So we are going to build two practical agents: **Post Generator** that drafts LinkedIn/X content using live web search & **Stack Analyzer** that inspects GitHub repos and creates structured reports.

We will be using Next.js frontend, FastAPI backend, [CopilotKit](https://go.copilotkit.ai/copilot), [LangGraph](https://www.langchain.com/langgraph) workflows, and [Google Gemini](https://gemini.google.com/). You will find architecture, concepts, prompts, and practical stuff.

Let's build it.

[Check out the CopilotKit GitHub ⭐️](https://go.copilotkit.ai/copilot)

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#1-what-are-we-building)1. What are we building?

We are building two practical agents using a full-stack setup:

✅ **Post Generator Agent** : creates LinkedIn/X posts grounded in live Google Search results.

Here's a simplified call sequence of what will happen when a user generates a post.  

```
[User types prompt]
     ↓
Next.js UI (CopilotChat)
     ↓ (POST /api/copilotkit → GraphQL)
Next.js API route (copilotkit)
     ↓ (forwards)
FastAPI backend (/copilotkit)
     ↓ (LangGraph workflow)
Post Generator graph nodes
     ↓ (calls → Google Gemini + web search)
Streaming responses & tool‑logs
     ↓
Frontend UI renders chat + tool logs + final postcards
```

✅ **Stack Analyzer Agent**: analyzes a public GitHub repo (metadata, README, code manifests) and infers its stack.

Here's a simplified call sequence of what will happen when a user analyzes the tech stack of a repo.  

```
[User pastes GitHub URL]
     ↓
Next.js UI (/stack‑analyzer)
     ↓
/api/copilotkit → FastAPI
     ↓
Stack Analysis graph nodes (gather_context → analyze → end)
     ↓
Streaming tool‑logs & structured analysis cards
```

Here's what we'll be building!

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#2-tech-stack-amp-architecture)2. Tech Stack & Architecture

At the core, we are going to use this stack for building these agents.

- [Next.js 15](https://nextjs.org/) : frontend framework with TypeScript
    
- [CopilotKit SDK](https://go.copilotkit.ai/copilot) : embed agents into UI (`@copilotkit/react-core`, `@copilotkit/runtime`, `@copilotkit/react-ui`)
    
- [FastAPI](https://fastapi.tiangolo.com/) & [Uvicorn](https://www.uvicorn.org/) : backend framework for serving agents as APIs
    
- [LangGraph (StateGraphs)](https://www.langchain.com/langgraph) : for building stateful agent workflows
    
- [Google Gemini via `google-genai`](https://github.com/googleapis/js-genai) (official SDK) : LLM for reasoning & text generation
    
- [LangChain’s Google adapter](https://python.langchain.com/docs/integrations/llms/google_ai/) : to plug Gemini into LangChain workflows
    
- [Pydantic](https://docs.pydantic.dev/latest/) : for structured JSON tool outputs
    

Here's the high-level architecture of the project.

|   |
|---|
||
|┌───────────────┐ GraphQL / HTTP ┌───────────────────┐|
|│ │ <——————— /api/copilotkit ————> │ │|
|│ Next.js │ │ FastAPI Agent │|
|│ Frontend │ │ (agent/main.py) │|
|│ (React + UI) │ │ │|
|└───────────────┘ └───────────────────┘|
|▲ ▲|
|│ │|
|│ │|
|│ │|
|/api/copilotkit route CopilotKitSDK|
|(CopilotKit Runtime endpoint) + LangGraphAgent workflows|
|▼|
|┌────────────────────┐|
|│ Gemini + Tools │|
|│ (Google Search) │|
|└────────────────────┘|

[view raw](https://gist.github.com/Anmol-Baranwal/8a833b19cc6a876296ca8df11731cbeb/raw/7483be99e3842eae8dbbbd7c4a8259495af1d405/architecture)[architecture](https://gist.github.com/Anmol-Baranwal/8a833b19cc6a876296ca8df11731cbeb#file-architecture) hosted with ❤ by [GitHub](https://github.com/)

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#project-structure)Project structure

This is how our directory will look. The `agent` directory will hold the Python/FastAPI backend hosting the LangGraph agents, and the `frontend` directory hosts the Next.js 15 application, including UI routes, API routes, and shared components.  

```
.
├── assets/                  
├── frontend/                ← Next.js 15 App (UI + API routes)
│   ├── app/
│   │   ├── layout.tsx       ← Wraps the app with `<CopilotKit>`
│   │   ├── post-generator/  ← Post Generator UI routes
│   │   ├── stack-analyzer/  ← Stack Analyzer UI routes
│   │   └── api/             ← Next.js API routes used by the UI
│   │   ...
│   ├── contexts/LayoutContext.tsx
│   ├── wrapper.tsx            ← CopilotKit provider wrapper
│   ├── components/          ← Shared UI components
│   │   ...
├── agent/                   ← FastAPI + LangGraph “agents” (Python)
│   ├── main.py              ← Registers agents and exposes them via FastAPI
│   ├── posts_generator_agent.py  ← Workflow for content creation agent
│   ├── stack_agent.py       ← Workflow for repo analysis agent
│   ├── prompts.py           ← Shared prompt templates
│   ├── agent.py             ← Core agent classes and helpers
│   ...
└── README.md                ← Project overview and setup instructions          
```

Here's the [GitHub repository](https://github.com/CopilotKit/CopilotKit-Deepmind) and deployed live at [copilot-kit-deepmind.vercel.app](https://copilot-kit-deepmind.vercel.app/) if you want to explore yourself. I will be covering the implementation with all the key concepts in the following sections.

The easiest way to follow along is to clone the repo but I'm explaining how to build it from scratch.  

```
git clone https://github.com/CopilotKit/CopilotKit-Deepmind.git
cd copilotkit-deepmind
```

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#add-necessary-api%C2%A0keys)Add necessary API Keys.

Create a `.env` file under both the `agent` & `frontend` directory and add your [Gemini API Key](https://aistudio.google.com/apikey) to the file. I've attached the docs link so it's easy to follow.

The naming convention is the same for both directories.  

```
GOOGLE_API_KEY=<<your-gemini-key-here>>
```

[![google gemini api key](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftf3sy64ruzpdctcgq3a7.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftf3sy64ruzpdctcgq3a7.png)

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#3-frontend)3. Frontend

Let's create the frontend. I'm attaching the project structure of the frontend again so it's easier for you to follow the whole layout.  

```
frontend/
├── app/
│   ├── page.tsx               ← landing redirect
│   ├── post‑generator/page.tsx← Post Generator UI
│   ├── stack‑analyzer/page.tsx← Stack Analyzer UI
│   ├── api/
│   │   ├── copilotkit/route.ts← CopilotKit router endpoint
│   │   └── chat/route.ts      ← OpenAI research demo
│   ├── contexts/LayoutContext.tsx
│   ├── wrapper.tsx            ← CopilotKit provider wrapper
│   └── prompts/prompts.ts     ← UI prompt templates
├── components/…                ← shared UI components (tool‑logs, cards, posts…)
└── layout.tsx, globals.css, etc.
```

If you don’t have a frontend, you can create a new Next.js app with TypeScript and then install the Copilotkit package. In the cloned repository, it’s already there, so you just need to install the dependencies using `pnpm i` under the `frontend` directory.  

```
// creates a nextjs app with typescript  
npx create-next-app@latest frontend
```

[![nextjs frontend](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F51fhtbamxap5kd1mgmhp.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F51fhtbamxap5kd1mgmhp.png)

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#step-1-copilotkit-provider-amp-layout)Step 1: CopilotKit Provider & Layout

Install the necessary CopilotKit packages.  

```
pnpm install copilotkit @copilotkit/react-core @copilotkit/react-ui @copilotkit/runtime @copilotkit/runtime-client-gql
```

- `copilotkit` is the lower-level SDK that bundles backend utilities for Python. Used here for wiring up state graphs, emitting state updates, and talking to Gemini.
    
- `@copilotkit/react-core` provides the core context and logic to connect your React app with the CopilotKit backend and MCP servers.
    
- `@copilotkit/react-ui` offers ready-made UI components like `<CopilotChat />` to build AI chat or assistant interfaces quickly.
    
- `@copilotkit/runtime` is the server-side runtime library. Let's you declare agents, connect them to LangGraph workflows, and expose them through an API endpoint.
    
- `@copilotkit/runtime-client-gql` is a client for GraphQL transport. Used under the hood by the Next.js API route to proxy between the browser and your backend.
    

[![install copilotkit packages](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxs87ycbylpo9fxsk2ddi.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxs87ycbylpo9fxsk2ddi.png)

The `<CopilotKit>` component must wrap the Copilot-aware parts of your application. In most cases, it's best to place it around the entire app, like in `layout.tsx`.

The root layout wraps everything in a LayoutProvider and the CopilotKit client wrapper:  

```
import "./globals.css"
import { LayoutProvider } from "./contexts/LayoutContext"
import Wrapper from "./wrapper"

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <LayoutProvider>
        <Wrapper>
          <body>{children}</body>
        </Wrapper>
      </LayoutProvider>
    </html>
  )
}
```

The LayoutProvider (`frontend\app\contexts\LayoutContext.tsx`) sets up a React context for layout state and picks the active agent based on the current route (`/post-generator` or others) using `usePathname()` to detect the path.  

```
"use client"

import { usePathname } from "next/navigation"
import React, { createContext, useContext, useState } from "react"

interface LayoutState { … }

interface LayoutContextType {
  layoutState: LayoutState
  updateLayout: (updates: Partial<LayoutState>) => void
}

const LayoutContext = createContext<LayoutContextType | undefined>(undefined)

const defaultLayoutState = { agent: "post_generation_agent", … }
export function LayoutProvider({ children }) {
  const pathname = usePathname()

  const [layoutState, setLayoutState] = useState({
    ...defaultLayoutState,
    agent: (pathname == "/post-generator"
      ? "post_generation_agent"
      : "stack_analysis_agent"),
  })

  const updateLayout = (updates) =>
    setLayoutState((prev) => ({ ...prev, ...updates }))

  return (
    <LayoutContext.Provider value={{ layoutState, updateLayout }}>
      {children}
    </LayoutContext.Provider>
  )
}

export function useLayout() {
  return useContext(LayoutContext)
}
...
```

Here's the code for the CopilotKit client wrapper (`frontend\app\wrapper.tsx`). Every page is rendered inside so that UI components know which agent to call and where.  

```
"use client"
import { CopilotKit } from "@copilotkit/react-core";
import { useLayout } from "./contexts/LayoutContext";

export default function Wrapper({ children }: { children: React.ReactNode }) {
  const { layoutState } = useLayout()
  return (
    <CopilotKit runtimeUrl="/api/copilotkit" agent={layoutState.agent}>
      {children}
    </CopilotKit>
  )
}
```

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#step-2-nextjs-api-routes-proxy-to-fastapi)Step 2: Next.js API Routes: Proxy to FastAPI

CopilotKit Runtime endpoint available at Next.js API route `app/api/copilotkit/route.ts` just proxies all agent/graph calls to the FastAPI backend.

Rather than calling the Python agent directly from the browser, we introduce a thin proxy.

Why?

- Avoid CORS and cross‑origin issues
- Let Next.js handle authentication, environment‑specific routing, and bundling
- Uniform GraphQL/REST shape for the React UI (no Python payloads leak into client)

In this example, we are only using a single agent, but if you are looking to run multiple LangGraph agents, check the [official Multi-Agent guide](https://docs.copilotkit.ai/coagents/multi-agent-flows).  

```
import { CopilotRuntime, copilotRuntimeNextJSAppRouterEndpoint, GoogleGenerativeAIAdapter } from "@copilotkit/runtime";
import { NextRequest } from "next/server";

// You can use any service adapter here for multi-agent support.
const serviceAdapter = new GoogleGenerativeAIAdapter();
const runtime = new CopilotRuntime({
  remoteEndpoints: [{ url: process.env.NEXT_PUBLIC_LANGGRAPH_URL || "http://localhost:8000/copilotkit" }],
});

export const POST = async (req: NextRequest) => {
  const { handleRequest } = copilotRuntimeNextJSAppRouterEndpoint({
    runtime,
    serviceAdapter,
    endpoint: "/api/copilotkit",
  });
  return handleRequest(req);
};
```

Here's a simple explanation of the above code:

- [`CopilotRuntime`](https://docs.copilotkit.ai/reference/classes/CopilotRuntime): the internal engine that connects your Copilot-enabled UI with agent endpoints.
    
- [`GoogleGenerativeAIAdapter`](https://docs.copilotkit.ai/reference/classes/llm-adapters/GoogleGenerativeAIAdapter): this adapter plugs in Google Gemini as the underlying LLM for agent workflows.
    
- `remoteEndpoints`: specifies where the agent logic lives (such as endpoint served by backend).
    
- `copilotRuntimeNextJSAppRouterEndpoint`: helper that wraps the incoming `req` and routes it to Copilot Runtime for agent processing. It returns a `handleRequest` method.
    

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#step-3-autoredirect-to-post-generator)Step 3: Auto‑Redirect to Post Generator

One last thing is to redirect to `/post-generator` route whenever someone hits home `/` route at `frontend\app\page.tsx`.  

```
"use client"

import "@copilotkit/react-ui/styles.css";
import { useEffect } from "react";
import { useRouter } from "next/navigation";
import { useLayout } from "./contexts/LayoutContext";
export default function GoogleDeepMindChatUI() {
  const router = useRouter();
  const { updateLayout } = useLayout();
  useEffect(() => {
    updateLayout({ agent: "post_generation_agent" });
    router.push("/post-generator");
  }, [router]);

  return (
    <></>
  )
}
```

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#step-4-post-generator-agent-ui)Step 4: Post Generator Agent UI

Let's create the frontend for Post Generator (`frontend/app/post-generator/page.tsx`) using the CopilotChat UI (`<CopilotChat>`), suggestions, and a custom action to render the final posts.

The real codebase also includes UI extras like agent switching, quick actions and live tool logs. For clarity, I have trimmed them here, so check the [code for full UI](https://github.com/CopilotKit/CopilotKit-Deepmind/blob/main/frontend/app/post-generator/page.tsx).  

```
import { CopilotChat, useCopilotChatSuggestions } from "@copilotkit/react-ui"
import { initialPrompt, suggestionPrompt } from "../prompts/prompts"

useCopilotChatSuggestions({
  available: "enabled",
  instructions: suggestionPrompt,
})

return (
  <div className="…">
    {/* …sidebar & header omitted… */}

    {/* Chat canvas */}
    <CopilotChat
      className="h-full p-2"
      labels={{ initial: initialPrompt }}
    />

    {/* Post previews (rendered after generation) */}
    <div className="flex gap-6 mt-6">
      <LinkedInPostPreview title="Generated Title" content="Generated LinkedIn content…" />
      <XPostPreview title="Generated Title" content="Generated X content…" />
    </div>
  </div>
)
```

The system & suggestion prompts come from `app/prompts/prompts.ts`.  

```
export const initialPrompt  = "Hi! I am a Langgraph x Gemini-powered AI agent capable of performing web search and generating LinkedIn and X (Twitter) posts.\n\n Click on the suggestions to get started."

export const suggestionPrompt = "Generate suggestions that revolve around the creation/generation of LinkedIn and X (Twitter) posts on any specific topics."
```

In the full UI code, we also use `useCopilotAction` to define a `generate_post` action. This is what lets the agent return structured LinkedIn/X posts, which then render into previews. For simplicity, here’s the trimmed code.  

```
import { useCopilotAction } from "@copilotkit/react-core"
import { XPostCompact, LinkedInPostCompact } from "@/components/ui/posts"

useCopilotAction({
  name: "generate_post",
  description: "Render a LinkedIn and X post",
  parameters: {
    tweet: { title: "string", content: "string" },
    linkedIn: { title: "string", content: "string" }
  },
  render: ({ args }) => (
    <>
      {args.tweet?.content && (
        <XPostCompact title={args.tweet.title} content={args.tweet.content} />
      )}
      {args.linkedIn?.content && (
        <LinkedInPostCompact title={args.linkedIn.title} content={args.linkedIn.content} />
      )}
    </>
  )
})
```

For debugging, we also render `tool_logs` with `useCoAgentStateRender`, which shows live tool invocations while the agent is working.  

```
import { useCoAgentStateRender } from "@copilotkit/react-core"
import { ToolLogs } from "@/components/ui/tool-logs"

useCoAgentStateRender({
  name: "post_generation_agent",
  render: (state) => (
    <ToolLogs logs={state?.state?.tool_logs || []} />
  )
})
```

Here's the final output of the code.

[![post generator ui](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgy0yvd3svjd1ngjuvums.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgy0yvd3svjd1ngjuvums.png)

I'm not covering the code for basic components like `Badge`, `textarea`, `x-post`, `linkedin-post`, and `button`. You can [check all the components in the repository](https://github.com/CopilotKit/CopilotKit-Deepmind/tree/main/frontend/components/ui) at `frontend/components/ui`.

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#step-5-stack-analyzer-agent-ui)Step 5: Stack Analyzer Agent UI

The stack‑analysis page (`frontend/app/stack-analyzer/page.tsx`) hooks into `stack_analysis_agent` and renders a set of cards. As previously, I have trimmed UI extras like agent switching, quick actions and live tool logs. You can check the [code for full UI](https://github.com/CopilotKit/CopilotKit-Deepmind/blob/main/frontend/app/stack-analyzer/page.tsx).

It's identical to what we did before, so I'm skipping the explanation of the code.  

```
import { CopilotChat, useCopilotChatSuggestions } from "@copilotkit/react-ui"
import { initialPrompt1, suggestionPrompt1 } from "../prompts/prompts"
import { StackAnalysisCards } from "@/components/ui/stack-analysis-cards"
import { ToolLogs } from "@/components/ui/tool-logs"

useCoAgentStateRender({
  name: "stack_analysis_agent",
  render: (state) => <ToolLogs logs={state?.state?.tool_logs || []} />,
})

useCopilotChatSuggestions({
  available: "enabled",
  instructions: suggestionPrompt1,
})

return (
  <div className="…">
    {/* …sidebar omitted… */}

    <CopilotChat
      className="h-full p-2"
      labels={{ initial: initialPrompt1 }}
    />

    {state.show_cards && <StackAnalysisCards analysis={state.analysis} />}
  </div>
)
```

The system & suggestion prompts come from `app/prompts/prompts.ts`.  

```
export const initialPrompt1 = 'Hi! I am a Langgraph x Gemini-powered AI agent capable of performing analysis of Public GitHub Repositories.\n\n Click on the suggestions to get started.'

export const suggestionPrompt1 = `Generate suggestions that revolve around the analysis of Public GitHub Repositories. Only provide suggestions from these public URLs:
[
  "https://github.com/freeCodeCamp/freeCodeCamp",
  "https://github.com/EbookFoundation/free-programming-books",
  "https://github.com/jwasham/coding-interview-university",
  "https://github.com/kamranahmedse/developer-roadmap",
  "https://github.com/public-apis/public-apis",
  "https://github.com/donnemartin/system-design-primer",
  "https://github.com/facebook/react",
  "https://github.com/tensorflow/tensorflow",
  "https://github.com/trekhleb/javascript-algorithms",
  "https://github.com/twbs/bootstrap",
  "https://github.com/vinta/awesome-python",
  "https://github.com/ohmyzsh/ohmyzsh",
  "https://github.com/tldr-pages/tldr",
  "https://github.com/ytdl-org/youtube-dl",
  "https://github.com/taigaio/taiga-back"
]`
```

Here's the final output of the code.

[![stack analyzer ui](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2im028ae76zrvqo5y4he.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2im028ae76zrvqo5y4he.png)

I'm not covering the code for basic components like `Badge`, `textarea`, `stack-analysis-cards`, and `button`. You can [check all the components in the repository](https://github.com/CopilotKit/CopilotKit-Deepmind/tree/main/frontend/components/ui) at `frontend/components/ui`.

---

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#4-backend-agent-service-fastapi-copilotkit-sdk)4. Backend Agent Service (FastAPI + CopilotKit SDK)

Under the `/agent` directory lives a FastAPI server that exposes two LangGraph‑based agents. Here's the project structure of the backend again, so it's easier for you to follow the whole layout.  

```
agent/
├── main.py                  ← FastAPI + CopilotKitSDK wiring
├── posts_generator_agent.py ← “Post Generator” graph & nodes
├── stack_agent.py           ← “Stack Analysis” graph & nodes
├── prompts.py               ← system prompts
├── pyproject.toml
└── agent.py                 ← Core agent classes and helpers
```

The backend uses [Poetry](https://python-poetry.org/docs/) instead of `requirements.txt`. Install it if you don't have it in your system.  

```
pip install poetry
```

Then, inside your `agent` directory, initialize a new Poetry project using the following command.  

```
cd agent
poetry init  # creates a pyproject.toml here (answer prompts or skip with --no-interaction)
```

This will generate a fresh `pyproject.toml` and `poetry.lock`, which means your backend now has its own virtual environment.

[![poetry init](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fst0l0blyrpdnz3btqser.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fst0l0blyrpdnz3btqser.png)

Most of the AI ecosystem (LangChain, LangGraph, Google SDKs) only supports up to Python 3.12 for now, so make sure to tell Poetry to use a compatible Python version by using this command: `poetry env use python3.12`.

[![check if python 3.12 is installed](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7vuoiawiwtbcy2xqs8f9.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7vuoiawiwtbcy2xqs8f9.png)

Then install the dependencies.  

```
poetry add fastapi uvicorn copilotkit langgraph langchain langchain-google-genai google-genai pydantic python-dotenv
```

[![install backend dependencies](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F02haw7avkg1bur5t3c1a.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F02haw7avkg1bur5t3c1a.png)

- `fastapi`: web framework for serving the agent endpoints (`/copilotkit`).
    
- `uvicorn`: the ASGI server used to run FastAPI in production or dev mode.
    
- `copilotkit`: the CopilotKit Python SDK that integrates LangGraph workflows with CopilotKit state streaming.
    
- `langgraph`: state-machine framework for defining agents as graphs of nodes (chat, analyze, end).
    
- `langchain`: provides core abstractions (`RunnableConfig`, message types, etc.) used inside nodes.
    
- `langchain-google-genai`: LangChain wrapper for Google Gemini models (e.g. `ChatGoogleGenerativeAI`).
    
- `google-genai`: official Google client SDK for Gemini, used for lower-level calls (e.g. `genai.Client`).
    
- `pydantic`: schema validation (`StructuredStackAnalysis`) to enforce strict JSON outputs.
    
- `python-dotenv` → loads `.env` files for managing API keys (like `GOOGLE_API_KEY`).
    

Now run the following command to generate a `poetry.lock` file pinned with exact versions.  

```
poetry install
```

[![poetry install](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhgibhllhezawi25kjxm7.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhgibhllhezawi25kjxm7.png)

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#fastapi-server-amp-sdk-setup)FastAPI Server & SDK Setup

All agents live behind a single FastAPI server (`agent/main.py`), which mounts them on `/copilotkit`.  

```
from fastapi import FastAPI
import uvicorn
from copilotkit.integrations.fastapi import add_fastapi_endpoint
from copilotkit import CopilotKitSDK, LangGraphAgent
from posts_generator_agent import post_generation_graph
from stack_agent import stack_analysis_graph

app = FastAPI()

sdk = CopilotKitSDK(
    agents=[
        LangGraphAgent(
            name="post_generation_agent",
            description="An agent that can help with the generation of LinkedIn posts and X posts.",
            graph=post_generation_graph,
        ),
        LangGraphAgent(
            name="stack_analysis_agent",
            description="Analyze a GitHub repository URL to infer purpose and tech stack (frontend, backend, DB, infra).",
            graph=stack_analysis_graph,
        ),
    ]
)

add_fastapi_endpoint(app, sdk, "/copilotkit")

# A simple endpoint to confirm the server is alive
@app.get("/healthz")
def health():
    return {"status": "ok"}

def main():
    """Run the uvicorn server."""
    port = int(os.getenv("PORT", "8000"))
    uvicorn.run(
        "main:app",
        host="0.0.0.0",
        port=port,
        reload=True,
    )

if __name__ == "__main__":
    main()
```

Here's what's happening behind the scenes:

- It spins up a FastAPI server
- Registers two LangGraph agents (`post_generation_agent`, `stack_analysis_agent`) inside CopilotKit
- Exposes them on `/copilotkit` so the frontend can talk to them
- Runs with Uvicorn

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#5-agent-workflows-langgraph-stategraphs)5. Agent Workflows (LangGraph StateGraphs)

Both agents are expressed as LangGraph state machines, stitched together with a few async nodes.

Every agent file (whether `posts_generator_agent.py` or `stack_agent.py`) follows the same LangGraph skeleton:

- Define a `StateGraph`
- Add nodes (each node = async function)
- Connect edges (`START → … → END`)
- Compile with `MemorySaver()`

But what changes is **what each node actually does**.

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#post-generator-graph)Post Generator Graph

The “Post Generator” workflow is defined in [`posts_generator_agent.py`](https://github.com/CopilotKit/CopilotKit-Deepmind/blob/main/agent/posts_generator_agent.py). It wires up three nodes (`chat_node`, `fe_actions_node`, `end_node`) into a compiled StateGraph.  

```
# Standard library
import os, uuid, asyncio
from typing import Dict, List, Any, Optional

# Environment
from dotenv import load_dotenv

# Google GenAI
from google import genai
from google.genai import types
from langchain_google_genai import ChatGoogleGenerativeAI

# LangGraph
from langgraph.graph import StateGraph, START, END
from langgraph.checkpoint.memory import MemorySaver
from langgraph.types import Command
from langchain_core.runnables import RunnableConfig
from langchain_core.messages import AIMessage

# CopilotKit
from copilotkit import CopilotKitState
from copilotkit.langgraph import copilotkit_emit_state
from copilotkit.langchain import copilotkit_customize_config

# Local
from prompts import system_prompt, system_prompt_3


load_dotenv()

class AgentState(CopilotKitState):
    tool_logs: List[Dict[str, Any]]
    response: Dict[str, Any]

# --- Nodes ---

async def chat_node(state: AgentState, config: RunnableConfig):

    model = genai.Client(api_key=os.getenv("GOOGLE_API_KEY"))
    state["tool_logs"].append(
        {
            "id": str(uuid.uuid4()),
            "message": "Analyzing the user's query",
            "status": "processing",
        }
    )
    await copilotkit_emit_state(config, state)

    if state["messages"][-1].type == "tool":
        client = ChatGoogleGenerativeAI(
            model="gemini-2.5-pro",
            temperature=1.0,
            max_retries=2,
            google_api_key=os.getenv("GOOGLE_API_KEY"),
        )
        messages = [*state["messages"]]
        messages[-1].content = (
            "The posts had been generated successfully. Just generate a summary of the posts."
        )
        resp = await client.ainvoke(
            [*state["messages"]],
            config,
        )
        state["tool_logs"] = []
        await copilotkit_emit_state(config, state)
        return Command(goto="fe_actions_node", update={"messages": resp})

    grounding_tool = types.Tool(google_search=types.GoogleSearch())
    model_config = types.GenerateContentConfig(
        tools=[grounding_tool],
    )
    # Define config for the model
    if config is None:
        config = RunnableConfig(recursion_limit=25)
    else:
        # Use CopilotKit's custom config functions to properly set up streaming
        config = copilotkit_customize_config(
            config, emit_messages=True, emit_tool_calls=True
        )

    # Bind the tools to the model
    response = model.models.generate_content(
        model="gemini-2.5-pro",
        contents=[
            types.Content(role="user", parts=[types.Part(text=system_prompt)]),
            types.Content(
                role="model",
                parts=[
                    types.Part(
                        text="I understand. I will use the google_search tool when needed to provide current and accurate information."
                    )
                ],
            ),
            types.Content(
                role="user", parts=[types.Part(text=state["messages"][-1].content)]
            ),
        ],
        config=model_config,
    )
    state["tool_logs"][-1]["status"] = "completed"
    await copilotkit_emit_state(config, state)
    state["response"] = response.text
    # Define the system message by which the chat model will be run
    for query in response.candidates[0].grounding_metadata.web_search_queries:
        state["tool_logs"].append(
            {
                "id": str(uuid.uuid4()),
                "message": f"Performing Web Search for '{query}'",
                "status": "processing",
            }
        )
        await asyncio.sleep(1)
        await copilotkit_emit_state(config, state)
        state["tool_logs"][-1]["status"] = "completed"
        await copilotkit_emit_state(config, state)
    return Command(goto="fe_actions_node", update=state)

async def fe_actions_node(state: AgentState, config: RunnableConfig):
    try:
        if state["messages"][-2].type == "tool":
            return Command(goto="end_node", update=state)
    except Exception as e:
        print("Moved")

    state["tool_logs"].append(
        {
            "id": str(uuid.uuid4()),
            "message": "Generating post",
            "status": "processing",
        }
    )
    await copilotkit_emit_state(config, state)
    model = ChatGoogleGenerativeAI(
        model="gemini-2.5-pro",
        temperature=1.0,
        max_retries=2,
        google_api_key=os.getenv("GOOGLE_API_KEY"),
    )
    await copilotkit_emit_state(config, state)
    response = await model.bind_tools([*state["copilotkit"]["actions"]]).ainvoke(
        [system_prompt_3.replace("{context}", state["response"]), *state["messages"]],
        config,
    )
    state["tool_logs"] = []
    await copilotkit_emit_state(config, state)
    return Command(goto="end_node", update={"messages": response})

async def end_node(state: AgentState, config: RunnableConfig):
    print("inside end node")
    return Command(goto=END, update={"messages": state["messages"], "tool_logs": []})

def router_function(state: AgentState, config: RunnableConfig):
    if state["messages"][-2].role == "tool":
        return "end_node"
    else:
        return "fe_actions_node"

# --- Graph wiring ---
workflow = StateGraph(AgentState)
workflow.add_node("chat_node", chat_node)
workflow.add_node("fe_actions_node", fe_actions_node)
workflow.add_node("end_node", end_node)
workflow.set_entry_point("chat_node")
workflow.set_finish_point("end_node")
workflow.add_edge(START, "chat_node")
workflow.add_edge("chat_node", "fe_actions_node")
workflow.add_edge("fe_actions_node", END)

post_generation_graph = workflow.compile(checkpointer=MemorySaver())
```

Here's the rough flow:

1. `chat_node`: invokes Google Gemini via `genai.Client`, optionally calls a web‑search tool, streams intermediate tool‑logs back to the UI
2. `fe_actions_node`: post‑processes the chat result to generate the final LinkedIn/X posts
3. `end_node`: finishes the workflow

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#stack-analysis-graph)Stack Analysis Graph

Similarly, the “Stack Analyzer” workflow is defined in `stack_agent.py`. It also wires up three nodes (`gather_context_node`, `analyze_with_gemini_node`, `end_node` ) into a compiled StateGraph.  

```
# OpenAI‑style tool that ensures the JSON schema is enforced
@tool("return_stack_analysis", args_schema=StructuredStackAnalysis)
def return_stack_analysis_tool(**kwargs) -> Dict[str, Any]:
    """Return the final stack analysis in strict JSON."""
    # …validate and return…
    validated = StructuredStackAnalysis(**kwargs)
    return validated.model_dump(exclude_none=True)

# ...
workflow = StateGraph(StackAgentState)
workflow.add_node("gather_context", gather_context_node)
workflow.add_node("analyze", analyze_with_gemini_node)
workflow.add_node("end", end_node)
workflow.add_edge(START, "gather_context")
workflow.add_edge("gather_context", "analyze")
workflow.add_edge("analyze", END)
workflow.set_entry_point("gather_context")
workflow.set_finish_point("end")

stack_analysis_graph = workflow.compile(checkpointer=MemorySaver())
```

Unlike Post Generator, this agent is much larger (~500 lines). Instead of pasting everything, I will walk through each node with key snippets.

You can check the repo for the [full implementation](https://github.com/CopilotKit/CopilotKit-Deepmind/blob/main/agent/stack_agent.py) (with retries, detailed logging, and schema validation).

Each node and what they actually do:

✅ 1. `gather_context_node`: this node parses the GitHub URL from the user’s message, fetches metadata via the GitHub API (repo info, languages, README, root files, manifests), and stores it in `state["context"]` for downstream analysis.  

```
async def gather_context_node(state: StackAgentState, config: RunnableConfig):
    last_user_content = state["messages"][-1].content if state["messages"] else ""
    parsed = _parse_github_url(last_user_content)

    if not parsed:
        return Command(goto="analyze", update={...})

    owner, repo = parsed
    repo_info = _fetch_repo_info(owner, repo)
    languages = _fetch_languages(owner, repo)
    readme = _fetch_readme(owner, repo)
    root_items = _list_root(owner, repo)
    manifests = _fetch_manifest_contents(owner, repo, repo_info.get("default_branch"), root_items)

    context = {"owner": owner, "repo": repo, "repo_info": repo_info,
               "languages": languages, "readme": readme,
               "root_files": _summarize_root_files(root_items),
               "manifests": manifests}

    return Command(goto="analyze", update={"context": context, ...})
```

✅ 2. `analyze_with_gemini_node`: builds a structured-output prompt from the repo context and asks Gemini (`gemini-2.5-pro`) to analyze it. Gemini is required to call the `return_stack_analysis` tool, which enforces a strict JSON schema.  

```
async def analyze_with_gemini_node(state: StackAgentState, config: RunnableConfig):
    prompt = _build_analysis_prompt(state["context"])
    messages = [
        SystemMessage(content="You are a senior software architect..."),
        HumanMessage(content=prompt),
    ]

    model = ChatGoogleGenerativeAI(model="gemini-2.5-pro", temperature=0.4, ...)
    bound = model.bind_tools([return_stack_analysis_tool])
    tool_msg = await bound.ainvoke(messages, config)

    # Extract structured payload (stack details)
    for call in getattr(tool_msg, "tool_calls", []):
        if call.get("name") == "return_stack_analysis":
            args = call.get("args", {})
            state["analysis"] = json.dumps(args)
            state["show_cards"] = True
```

✅ 3. `end_node`: this final node clears tool logs and emits the finished analysis back to the UI.  

```
async def end_node(state: StackAgentState, config: RunnableConfig):
    state["tool_logs"] = []
    await copilotkit_emit_state(config or RunnableConfig(recursion_limit=25), state)
    return Command(goto=END, update={
        "messages": state["messages"],
        "show_cards": state["show_cards"],
        "analysis": state["analysis"]
    })
```

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#6-prompts-amp-tooling)6. Prompts & Tooling

Before wiring up graphs and nodes, agents rely heavily on `prompts and tools`. Prompts define how the model should behave (such as “always use Google search” or “generate posts in LinkedIn style”), while tools provide structured ways to capture output.

Let's cover the core building blocks that both agents share: system prompts, structured-output tools, and helper functions for constructing analysis prompts.

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#system-prompts-for-post-generation)✅ System prompts for post generation

All the “system & user prompt” templates for the Post Generator live in `agent/prompts.py`. These templates act as the persona of the agent.

Keeping prompts in a different file makes them easy to tweak independently of the workflow logic.  

```
system_prompt = """You have access to a google_search tool …
You MUST ALWAYS use the google_search tool for EVERY query…"""

system_prompt_2 = """
You are an Amazing artist. You need to generate an image …
"""

system_prompt_3 = """
You are an amazing assistant. You are familiar with the LinkedIn and X (Twitter) algorithms…
Always use the generate_post tool to generate the post.
{context}
"""
```

**How it is used:**

- `system_prompt` is injected inside the `chat_node`, forcing Gemini to ground answers using the `google_search` tool.
    
- `system_prompt_3` is consumed inside the `fe_actions_node` to tell Gemini how to frame LinkedIn/X posts.
    

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#building-the-stackanalysis-prompt)✅ Building the stack‑analysis prompt

In the Stack Analyzer, we use a helper function to inject GitHub repo context into a single “analyze the stack” prompt. This lives in `agent/stack_agent.py`.

Unlike prompts, this helper is tightly coupled to the stack analysis logic (schema, context parsing), so it's in the same agent file.  

```
def _build_analysis_prompt(context: Dict[str, Any]) -> str:
    return (
        "You are a senior software architect. Analyze the following GitHub repository at a high level.\n"
        "Goals: Provide a concise, structured overview of what the project does and the tech stack.\n\n"
        f"Repository metadata:\n{json.dumps(context['repo_info'], indent=2)}\n\n"
        f"Languages:\n{json.dumps(context['languages'], indent=2)}\n\n"
        "Root items:\n" + json.dumps(context['root_files'], indent=2) + "\n\n"
        "README content (truncated):\n" + context["readme"][:8000] + "\n\n"
        "Infer the stack with specific frameworks and libraries when possible…"
    )
```

**How it is used:**

- `_build_analysis_prompt` is passed into Gemini inside `analyze_with_gemini_node`, feeding a consolidated view of repo metadata, languages, manifests, and README.

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#structuredoutput-tool-for-stack-analysis)✅ Structured‑output tool for stack analysis

In `stack_agent.py`, we declare an OpenAI-style tool that enforces JSON output.  

```
@tool("return_stack_analysis", args_schema=StructuredStackAnalysis)
def return_stack_analysis_tool(**kwargs) -> Dict[str, Any]:
    """Return the final stack analysis in a strict JSON structure."""
    validated = StructuredStackAnalysis(**kwargs)
    return validated.model_dump(exclude_none=True)
```

**How it is used:**

- `return_stack_analysis_tool` is bound to Gemini inside `analyze_with_gemini_node`, so it must output JSON instead of free-form text.

The schema ensures every repo analysis has the same shape, which the UI can reliably display.

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#7-complete-flow)7. Complete flow

This is how the end-to-end data flow looks once we have integrated all the parts. It would be easier to understand if you had followed up with the blog.

|   |
|---|
|┌────────────────┐|
|│ Browser UI │|
|│ (CopilotChat) │|
|└───────┬────────┘|
|│ POST /api/copilotkit (GraphQL)|
|▼|
|┌────────────────────────────────────────────────┐|
|│ Next.js API Route │|
|│ (copilotRuntimeNextJSAppRouterEndpoint) │|
|└───────┬────────────────────────────────────────┘|
|│ proxies to|
|▼|
|┌────────────────────────────────────────────────┐|
|│ FastAPI + CopilotKitSDK │|
|│ (agent/main.py) │|
|│ • pick agent by name │|
|│ • feed LangGraphAgent → graph.run() │|
|└───────┬────────────────────────────────────────┘|
|│ invokes|
|▼|
|┌────────────────────────────────────────────────┐|
|│ LangGraph StateGraph │|
|│ • chat_node / gather_context_node… │|
|│ • streams intermediate state via │|
|│ copilotkit_emit_state() │|
|└───────┬────────────────────────────────────────┘|
|│ calls|
|▼|
|┌────────────────────────────────────────────────┐|
|│ Google Gemini (LLM) + google_search tool │|
|│ (ChatGoogleGenerativeAI) │|
|│ • generate_content() │|
|└───────┬────────────────────────────────────────┘|
|│ streaming responses & tool_calls|
|▼|
|┌────────────────────────────────────────────────┐|
|│ CopilotKitSDK streams back messages, logs… │|
|└───────┬────────────────────────────────────────┘|
|│ consumed by|
|▼|
|┌─────────────────┐|
|│ Browser UI │|
|│ (renders chat, │|
|│ tool‑logs, │|
|│ post cards or │|
|│ analysis cards)│|
|└─────────────────┘|

[view raw](https://gist.github.com/Anmol-Baranwal/3bbd667d43e8656a4d289b676bfc7e8d/raw/bc3af08ba96a30e479032218ef7d24364bd4b205/complete%20flow)[complete flow](https://gist.github.com/Anmol-Baranwal/3bbd667d43e8656a4d289b676bfc7e8d#file-complete-flow) hosted with ❤ by [GitHub](https://github.com/)

---

## [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#8-the-final-demo)8. The Final Demo

After completing all the parts of the code, it's time to run it locally. Please make sure you have added the Google Gemini credentials to the `.env` file.

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#start-backend-fastapi-agent)Start Backend (FastAPI agent)

Run the following commands in the `agent` directory.  

```
cd agent
poetry install
# set GOOGLE_API_KEY in agent/.env
poetry run python main.py
```

[![running backend locally](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fqf1mjt0rkxu84zzc4nap.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fqf1mjt0rkxu84zzc4nap.png)

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#start-frontend)Start Frontend

Run the following command to start the server locally under `frontend` and navigate to [localhost:3000/copilotkit](http://localhost:3000/copilotkit) in your browser to view your frontend.  

```
cd frontend
pnpm install # if you have cloned the repo
pnpm run dev
```

[![frontend running locally](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4q9qd3gdvmhwxoaqdcfn.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F4q9qd3gdvmhwxoaqdcfn.png)

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#output-of-post-generator-agent)🎯 Output of Post Generator Agent

The default route will lead to the `post-generator` agent. As you can see, it's properly generating the posts with deep research.

It emits intermediate “tool‑logs” so the UI shows each research/search/generation step in real time & you can also find some pre‑built starter prompts to get going with one click.

### [](https://dev.to/copilotkit/heres-how-to-build-fullstack-agent-apps-gemini-copilotkit-langgraph-15jb#output-of-stack-analyzer-agent)🎯 Output of Stack Analyzer Agent

It analyzes a public GitHub repo (metadata, README, code manifests) and infers its stack.

As I mentioned previously, it uses a Pydantic data model (`StructuredStackAnalysis`) to enforce a strictly defined JSON output covering:

- Project purpose
- Frontend stack (framework/language/libs)
- Backend stack (framework/lang/libs/architecture)
- Database details
- Infrastructure/hosting
- CI/CD setup
- Key root files
- How‑to‑run instructions
- Risk/notes sections

Similar to the Post Generator, it streams each step (URL parsing → fetching metadata → analyzing → summarizing) back to the UI.

---

That's it. The patterns users here (stateful graphs, tool bindings, structured outputs) will save you hours.

I hope you found something valuable in this hands-on guide. If you have built something before, please share in the comments.