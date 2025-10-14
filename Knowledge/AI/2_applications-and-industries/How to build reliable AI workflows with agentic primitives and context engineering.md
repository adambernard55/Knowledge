Skip to content
/
Blog


Home / AI & ML / GitHub Copilot
How to build reliable AI workflows with agentic primitives and context engineering
See how this three-part framework will turn AI into a repeatable and reliable engineering practice.

Header image with a GitHub Copilot character and the text "GitHub Copilot: Working with agentic primitives."
Daniel Meppiel·@danielmeppiel
October 13, 2025
16 minutes
Share:
Many developers begin their AI explorations with a prompt. Perhaps you started the same way: You opened GitHub Copilot, started asking questions in natural language, and hoped for a usable output. This approach can work for simple fixes and code suggestions, but as your needs get more complex—or as your work gets more collaborative—you’re going to need a more foolproof strategy. 

This guide will introduce you to a three-part framework that transforms this ad-hoc style of AI experimentation into a repeatable and reliable engineering practice. At its core are two concepts: agentic primitives, which are reusable, configurable building blocks that enable AI agents to work systematically; and context engineering, which ensures your AI agents always focus on the right information. By familiarizing yourself with these concepts, you’ll be able to build AI systems that can not only code independently, but do so reliably, predictably, and consistently.

An AI-native development framework, showing spec-driven development and agent workflows at the top, context engineering (including roles, rules, context, and memory) below, and prompt engineering (including role activation, context loading, tool invocation, and validation gates) at the base.
The AI-native development framework
Markdown prompt engineering + agent primitives + context engineering = reliability
Whether you’re new to AI-native development or looking to bring deeper reliability to your agent workflows, this guide will give you the foundation you need to build, scale, and share intelligent systems that learn and improve with every use.

🧠 Try it yourself: Build and run agentic workflows with GitHub Copilot CLI
Bring your agent primitives to life right from your terminal. The new GitHub Copilot CLI lets you run, debug, and automate AI workflows locally—no setup scripts, no context loss. It connects directly to your repositories, pull requests, and issues through GitHub MCP, giving your agents the same context they’d have in your IDE. 

👉 Get started with GitHub Copilot CLI >

What are agent primitives? 
The three-layer framework below turns ad-hoc AI experimentation into a reliable, repeatable process. It does this by combining the structure of Markdown; the power of agent primitives, simple building blocks that give your AI agents clear instructions and capabilities; and smart context management, so your agents always get the right information (not just more information). 

Layer 1: Use Markdown for more strategic prompt engineering
We’ve written about the importance of prompt engineering. But here’s what you need to know: The clearer, more precise, more context-rich your prompt, the better, more accurate your outcome. This is where Markdown comes in. With Markdown’s structure (its headers, lists, and links), you can naturally guide AI’s reasoning, making outputs more predictable and consistent. 

To provide a strong foundation for your prompt engineering, try these techniques with Markdown as your guide: 

Context loading: [Review existing patterns](./src/patterns/). In this case, links become context injection points that pull in relevant information, either from files or websites.
Structured thinking: Use headers and bullets to create clear reasoning pathways for the AI to follow.
Role activation: Use phrases like “You are an expert [in this role].” This triggers specialized knowledge domains and will focus the AI’s responses.
Tool integration: Use MCP tool tool-name. This lets your AI agent run code in a controlled, repeatable, and predictable way on MCP servers.
Precise language: Eliminate ambiguity through specific instructions.
Validation gates: “Stop and get user approval.” Make sure there is always human oversight at critical decision points.
For example, instead of saying, Find and fix the bug, use the following:

You are an expert debugger, specialized in debugging complex programming issues.

You are particularly great at debugging this project, which architecture and quirks can be consulted in the [architecture document](./docs/architecture.md). 

Follow these steps:

1. Review the [error logs](./logs/error.log) and identify the root cause. 

2. Use the `azmcp-monitor-log-query` MCP tool to retrieve infrastructure logs from Azure.  

3. Once you find the root cause, think about 3 potential solutions with trade-offs

4. Present your root cause analysis and suggested solutions with trade-offs to the user and seek validation before proceeding with fixes - do not change any files.
Once you’re comfortable with structured prompting, you’ll quickly realize that manually crafting perfect prompts for every task is unsustainable. (Who has the time?) This is where the second step comes in: turning your prompt engineering insights into reusable, configurable systems.

Layer 2: Agentic primitives: Deploying your new prompt engineering techniques
Now it’s time to implement all of your new strategies more systematically, instead of prompting ad hoc. These configurable tools will help you do just that.

Core agent primitives
When it comes to AI-native development, a core agent primitive refers to a simple, reusable file or module that provides a specific capability or rule for an agent. 

Here are some examples:

Instructions files: Deploy structured guidance through modular .instructions.md files with targeted scope. At GitHub, we offer custom instructions to give Copilot repository-specific guidance and preferences. 
Chat modes: Deploy role-based expertise through .chatmode.md files with MCP tool boundaries that prevent security breaches and cross-domain interference. For example, professional licenses that keep architects from building and engineers from planning.
Agentic workflows: Deploy reusable prompts through .prompt.md files with built-in validation.
Specification files: Create implementation-ready blueprints through .spec.md files that ensure repeatable results, whether the work is done by a person or by AI.
Agent memory files: Preserve knowledge across sessions through .memory.md files.
Context helper files: Optimize information retrieval through .context.md files.
How using a core agent primitive can transform a prompt and its outcome
Technique: Using Markdown prompt engineering, your prompt can be: “Implement secure user authentication system” 

Primitives: You’ll select backend-dev chat mode → Auto-triggers security.instructions.md via applyTo: "auth/**" → Loads context from [Previous auth patterns](.memory.md#security) and [API Security Standards](api-security.context.md#rest) → Generates user-auth.spec.md using structured templates → Executes implement-from-spec.prompt.md workflow with validation gates.

Outcome: Developer-driven knowledge accumulation where you capture implementation failures in .memory.md, document successful patterns in .instructions.md, and refine workflows in .prompt.md files—creating compound intelligence that improves through your iterative refinement.

This transformation might seem complex, but notice the pattern: What started as an ad-hoc request became a systematic workflow with clear handoff points, automatic context loading, and built-in validation. 

When you use these files and modules, you can keep adjusting and improving how your AI agent works at every step. Every time you iterate, you make your agent a little more reliable and consistent. And this isn’t just random trial and error — you’re following a structured, repeatable approach that helps you get better and more predictable results every time you use the AI.

💡 Native VS Code support: While VS Code natively supports .instructions.md, .prompt.md, and .chatmode.md files, this framework takes things further with .spec.md, .memory.md, and .context.md patterns that unlock even more exciting possibilities AI-powered software development.
With your prompts structured and your agentic primitives set up, you may encounter a new challenge: Even the best prompts and primitives can fail when they’re faced with irrelevant context or they’re competing for limited AI attention. The third layer, which we’ll get to next, addresses this through strategic context management.

Layer 3: Context engineering: Helping your AI agents focus on what matters
Just like people, LLMs have finite limited memory (context windows), and can sometimes be forgetful. If you can be strategic about the context you give them, you can help them focus on what’s relevant and enable them to get started and work quicker. This helps them preserve valuable context window space and improve their reliability and effectiveness.

Here are some techniques to make sure they get the right context—this is called context engineering: 

Session splitting: Use distinct agent sessions for different development phases and tasks. For example, use one session for planning, one for implementation, and one for testing. If an agent has fresh context, it’ll have better focus. It’s always better to have a fresh context window for complex tasks. 
Modular and custom rules and instructions: Apply only relevant instructions through targeted .instructions.md files using applyTo YAML frontmatter syntax. This preserves context space for actual work and reduces irrelevant suggestions. 
Memory-driven development: Leverage agent memory through .memory.md files to maintain project knowledge and decisions across sessions and time.
Context optimization: Use .context.md context helper files strategically to accelerate information retrieval and reduce cognitive load. 
Cognitive focus optimization: Use chat modes in .chatmode.md files to keep the AI’s attention on relevant domains and prevent cross-domain interference. Less context pollution means you’ll have more consistent and accurate outputs. 
Agentic workflows: The complete system in action
Now that you understand all three layers, you can see how they combine into agentic workflows—complete, systematic processes where all of your agentic primitives are working together, understanding your prompts, and using only the context they need.  

These agentic workflows can be implemented as .prompt.md files that coordinate multiple agentic primitives into processes, designed to work whether executed locally in your IDE, in your terminal or in your CI pipelines.

Need a recap? 
Markdown prompt engineering provides the structural foundation for predictable AI interactions.
Agent primitives are your configurable tools that scale and systematize these techniques.
Context engineering optimizes AI cognitive performance within memory constraints.
Agentic workflows in Markdown apply prompt and context engineering that leverages agent primitives to implement complete, reliable agentic processes.
This framework creates compound intelligence that improves as you continue to iterate.
Tooling: how to scale agent primitives
Now that you understand the three-layer framework and that the agentic primitives are essentially executable software written in natural language, the question is: How can you scale these Markdown files beyond your individual development workflow?

Natural language as code
The answer mirrors every programming ecosystem’s evolution. Just like JavaScript evolved from browser scripts to using Node.js runtimes, package managers, and deployment tooling, agent primitives need similar infrastructure to reach their full potential.

This isn’t just a metaphor: These .prompt.md and .instructions.md files represent a genuine new form of software development that requires proper tooling infrastructure.

Here’s what we mean: Think of your agent primitives as real pieces of software, just written in natural language instead of code. They have all the same qualities: You can break complex tasks into smaller pieces (modularity), use the same instructions in multiple places (reusability), rely on other tools or files (dependencies), keep improving and updating them (evolution), and share them across teams (distribution).

That said, your natural language programs are going to need the same infrastructure support as any other software.  

Agent CLI runtimes
Most developers start by creating and running agent primitives directly in VS Code with GitHub Copilot, which is ideal for interactive development, debugging, and refining daily workflows. However, when you want to move beyond the editor—to automate your workflows, schedule them, or integrate them into larger systems—you need agent CLI runtimes like Copilot CLI. 

These runtimes let you execute your agent primitives from the command line and tap into advanced model capabilities. This shift unlocks automation, scaling, and seamless integration into production environments, taking your natural language programs from personal tools to powerful, shareable solutions. 

Inner loop vs. outer loop
Inner loop (VS Code and GitHub Copilot): Interactive development, testing, and workflow refinement
Outer loop (agent CLI runtimes): Reproducible execution, CI/CD integration, and production deployment
Agent CLI Runtimes transform your agent primitives from IDE-bound files into independently executable workflows that run consistently across any environment. They provide command-line execution, CI/CD integration, environment consistency, and native support for MCP servers, which bridge your development work to production reality.

TL;DR: Use the inner loop for rapid, interactive work and the outer loop for reliable, repeatable automation and deployment.

Runtime management
While VS Code and GitHub Copilot handle individual development, some teams may want additional infrastructure for sharing, versioning, and productizing their agent primitives. Managing multiple Agent CLI runtimes can become complex quickly, with different installation procedures, configuration requirements, and compatibility matrices.

APM (Agent Package Manager) solves this by providing unified runtime management and package distribution. Instead of manually installing and configuring each vendor CLI, APM handles the complexity while preserving your existing VS Code workflow.

Here’s how runtime management works in practice:

# Install APM once
curl -sSL https://raw.githubusercontent.com/danielmeppiel/apm/main/install.sh | sh

# Optional: setup your GitHub PAT to use GitHub Copilot CLI
export GITHUB_COPILOT_PAT=your_token_here

# APM manages runtime installation for you
apm runtime setup copilot          # Installs GitHub Copilot CLI
apm runtime setup codex            # Installs OpenAI Codex CLI

# Install MCP dependencies (like npm install)
apm install

# Compile Agent Primitive files to Agents.md files
apm compile

# Run workflows against your chosen runtime
# This will trigger 'copilot -p security-review.prompt.md' command 
# Check the example apm.yml file a bit below in this guide
apm run copilot-sec-review --param pr_id=123
As you can see, your daily development stays exactly the same in VS Code, APM installs and configures runtimes automatically, your workflows run regardless of which runtime is installed, and the same apm run command works consistently across all runtimes.

Distribution and packaging
Agent primitives’ similarities to traditional software become most apparent when you get to the point of wanting to share them with your team or deploying them into production—when you start to require things like package management, dependency resolution, version control, and distribution mechanisms.

Here’s the challenge: You’ve built powerful agent primitives in VS Code and your team wants to use them, but distributing Markdown files and ensuring consistent MCP dependencies across different environments becomes unwieldy. You need the equivalent of npm for natural language programs.

APM provides this missing layer. It doesn’t replace your VS Code workflow—it extends it by creating distributable packages of agent primitives complete with dependencies, configuration, and runtime compatibility that teams can share, just like npm packages.

Package management in practice
# Initialize new APM project (like npm init)
apm init security-review-workflow

# Develop and test your workflow locally
cd security-review-workflow 
apm compile && apm install
apm run copilot-sec-review --param pr_id=123

# Package for distribution (future: apm publish)
# Share apm.yml and Agent Primitive files with team
# Team members can install and use your primitives
git clone your-workflow-repo
cd your-workflow-repo && apm compile && apm install
apm run copilot-sec-review --param pr_id=456
The benefits compound quickly: You can distribute tested workflows as versioned packages with dependencies, automatically resolve and install required MCP servers, track workflow evolution and maintain compatibility across updates, build on (and contribute to) shared libraries from the community, and ensure everyone’s running the same thing.

Project configuration
The following  apm.yml configuration file serves as the package.json equivalent for agent primitives, defining scripts, dependencies, and input parameters:

# apm.yml - Project configuration (like package.json)
name: security-review-workflow
version: 1.2.0
description: Comprehensive security review process with GitHub integration

scripts:
  copilot-sec-review: "copilot --log-level all --log-dir copilot-logs --allow-all-tools -p security-review.prompt.md"
  codex-sec-review: "codex security-review.prompt.md"
  copilot-debug: "copilot --log-level all --log-dir copilot-logs --allow-all-tools -p security-review.prompt.md"
  
dependencies:
  mcp:
    - ghcr.io/github/github-mcp-server
With this, your agent primitives can now be packaged as distributable software with managed dependencies.

Production deployment
The final piece of the tooling ecosystem enables continuous AI: packaged agent primitives can now run automatically in the same CI/CD pipelines you use every day, bringing your carefully developed workflows into your production environment.

Using APM GitHub Action, and building on the security-review-workflow package example above, here’s how the same APM project deploys to production with multi-runtime flexibility:

# .github/workflows/security-review.yml
name: AI Security Review Pipeline
on: 
  pull_request:
    types: [opened, synchronize]

jobs:
  security-analysis:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        # Maps to apm.yml scripts
        script: [copilot-sec-review, codex-sec-review, copilot-debug]  
    permissions:
      models: read
      pull-requests: write
      contents: read
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Run Security Review (${{ matrix.script }})
      uses: danielmeppiel/action-apm-cli@v1
      with:
        script: ${{ matrix.script }}
        parameters: |
          {
            "pr_id": "${{ github.event.pull_request.number }}"
          }
      env:
        GITHUB_COPILOT_PAT: ${{ secrets.COPILOT_CLI_PAT }}
Key connection: The matrix.script values (copilot-sec-review, codex-sec-review, copilot-debug) correspond exactly to the scripts defined in the apm.yml configuration above. APM automatically installs the MCP dependencies (ghcr.io/github/github-mcp-server) and passes the input parameters (pr_id) to your security-review.prompt.md workflow.

Here’s why this matters: 

Automation: Your AI workflows now run on their own, without anyone needing to manually trigger them.
Reliability: They run with the same consistency and reproducibility as traditional code deployments.
Flexibility: You can run different versions or types of analysis (mapped to different scripts) as needed.
Integration: These w