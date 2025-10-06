

By 2026, AI agents will run workflows — but only if we stop chasing 'super agents' and design them to stay in their lanes.

In 2023, chatbots answered questions. By 2025, AI agents can code and design entire applications and services from scratch, as well as do deep, nearly scientific-grade research on any topic. Now, as enterprises deploy armies of autonomous agents, a critical question emerges: How do we prevent these powerful tools from descending into chaos in the coming years? We at Trevolution chose not to restrain our ambition but redesign it instead.

Our own journey in developing AI in 2023 had a rocky start: We were building and testing a chatbot, Olivia, for customer support. It could answer simple questions — think along the lines of early ChatGPT functionality; nothing but a chatbot. It sounded good in theory; however, our market analysis indicated that the real-world application would have limited utility. Our analysis revealed that customers in travel don’t contact support to chat — they require specific actions to be performed. Industry experience had shown that customers typically expect support systems to handle actionable requests: rebooking flights, fixing reservations and processing ticket refund inquiries. However, Olivia functioned solely as a conversational chatbot and lacked the capability to execute these operational tasks, which can only be performed by trained customer service agents with appropriate system access. 

Following this assessment, we decided to reorient our approach, focusing on internal AI applications: testing how Olivia could assist employees rather than customers. This approach also offered reduced complexity, more structured feedback mechanisms and a controlled operational scope. By late 2023, Olivia had been developed as an AI assistant with clearly defined responsibilities and demonstrated consistent performance in controlled testing environments according to established metrics, though we knew it was capable of so much more…


No turning back
Then came the industry switch, which followed two key events: OpenAI announcing agentic AI as a core direction in March this year (having previously released Swarm in October 2024). And Model Context Protocol (MCP) being released by Anthropic back in November 2024 to minimal initial fanfare — now transformed into the de facto industry standard.

AI Agents weren’t science fiction anymore. Suddenly, they became reality, so we started developing an agentic platform immediately. Not just human-to-agent interaction. Agent-to-agent communication using Google’s A2A protocol. The goal? A specialized team where each AI agent does one thing perfectly and, together, they handle complex workflows. Imagine a workforce where one agent summarizes meetings. Another books flights. A third analyzes customer calls. All working in unison.

Most companies get this wrong. Lured by the marketing talk of third-party vendors and the grand promise of AI as an answer to all their problems, they try building monolithic agents — jacks-of-all-trades. But they often become haunted by hallucinations — the stronger they are, the harder they fall.

Specialize or fail
Why are specialized-niche AI agents superior? They don’t create chaos when they fail. Imagine this: A YouTube summarization agent with the explicit task of only summarizing YouTube videos. If you give it a BBC documentary, it should simply say: “This isn’t YouTube.” It does not hallucinate or, God forbid, attempt any creative solutions. When it fails, it does so cleanly. That’s control.

Whereas one agent doing everything only invites disaster. Unlimited failure points. Unlimited hallucinations.

So instead of building massive AI agents, build agentic pyramids, as suggested by Microsoft and OpenAI:

Base layer: Micro-agents with atomic functions (transcriber, Jira ticket fetcher, flight rebooker)
Middle: Tool integrators (MCP servers with precise, surgical permissions)
Apex: Orchestrator agents (split tasks, manage fallback, escalate to humans)
Essentially, the orchestrator handles tasks like a project manager. It can answer questions like “What’s the AI agent team’s top priority?” It delegates, for instance, a Jira agent to pull tickets or statistics or a call analytics agent to examine customer pain points, or a translation agent to process foreign-language feedback. The orchestrator assembles answers without any single agent overstepping their predefined bounds. If, however, one of the systems is down, it doesn’t create absolute hallucination mayhem down the line.

CIO Smart Answers Learn more
Explore related questions
How does Model Context Protocol (MCP) enable AI agents?
What are AI teams' challenges deploying and scaling agentic AI systems?
Why are specialized AI agents superior to monolithic agents?
Can AI agents use external software tools today?
What are security risks of autonomous AI agents accessing business systems?
Ask a question

Ask
This structure of an AI agent is very similar to the micro-service architecture in traditional software design — most principles from micro services can be applied to the agentic architecture.

Tools are your kill switch
Another way to guarantee success in creating your AI agent fleet: Forget controlling the agents themselves; control their tools instead. Essentially, MCP servers enable what your agents can do. So let’s say a tool has the ability to delete all JIRA tickets. If it is so, then it eventually will happen — eventually, one agent will hallucinate and will delete everything. Think of it as Murphy’s Law of AI hallucination inevitability — it’s not a question of if, it’s a question of when.

So actual agentic AI security isn’t about the LLMs, it’s about the tools. Take MCP for GitLab  as an example. Proper security isn’t about configuring the LLM through a system prompt — it’s about setting up access rights correctly within MCP itself. Murphy’s Law says: Anything that can go wrong will go wrong. So if MCP allows undesirable actions — deleting code, modifying the repository and so on — you can be sure they will eventually happen. Real security comes from giving the agent (via MCP) only the minimum permissions it truly needs.

What’s the worst possible action this enables?
What permissions can we amputate?
How do we log every interaction?
At Trevolution, we follow the concept of minimum required permissions. Greedy tools create reckless agents. Consider, for instance, what might happen if we gave an AI agent unnecessary write access to rewrite flight-pricing algorithms. Think the 2024 CrowdStrike IT outage but on steroids. The potential damage would take days — if not weeks — to fix.

Fallback isn’t optional
Agents fail. So IT leadership should plan for it, because happy-path testing kills systems. Test the ugly paths.

Agents must communicate failures instantly. Using A2A protocols, they signal the orchestrator: “Can’t handle this.” The orchestrator reroutes or escalates to their human counterparts. No silent errors. No guessing.

Take meeting summarization. A proper agent needs three tools: meeting audio extraction, speech-to-text service and summarization engine. Now, if speech-to-text fails, the agent reports: “Audio processing unavailable.” The orchestrator routes the task to a human. Clean. Predictable.

The hard part
Here’s what nobody tells you: Setting up the AI agents themselves is relatively easy. Any developer can create an agent with a few good prompts. Crafting their tools — building the MCP servers? Also not rocket science. But the hard part is making the agent work (reliably) with the MCP server.

We prioritize tools using a simple but brutal matrix:

Vertical axis: Implementation ease (i.e., can we use GitLab’s MCP?)
Horizontal axis: Business impact (i.e., will this automate 30% of manual work?)
High-impact, easy-win tools should get built first. Think of a confluence search agent able to read documentation and answer employee questions. Impact: Massive. Implementation: Atlassian’s ready-made MCP.

A custom flight-booking tool? Different story. Some time to build the MCP server. Another week for safety reviews. Result: An agent that can check flight availability but not book tickets. Why? Because giving it booking access for now would be greedy. Unnecessarily reckless.

The writing on the wall
By early 2026, AI agents will write their own tools. Scary? Only if you’re unprepared. 


Agent identifies a missing capability (e.g., “Need Instagram video summarization”)
Agent writes Python code for an Instagram API tool
Agent adds new code to its available tools
Something similar is predicted in this beautiful timeline, called “Self-improving AI“. For now, it was a supervised experience. In 2026, it will be possible to be done with no human involvement whatsoever.

2025 action plan for CIOs
Shatter monolithic AI agents into micro-specialists. One agent, one task.
Handcuff your tools. Minimum required permissions first. Additional access only after a ten-times review. Allow delete access — almost never.
Deploy orchestrators as central nervous systems. They handle task-splitting, failure routing, human escalation.
Log everything: every agent action, every tool usage, every failure. Compared to bankruptcy, storage is cheap.
Long story short: Stop chasing super agents and your agent workforce will only be as reliable as your tools are constrained. Chaos isn’t inevitable; it’s a design flaw. Tame it through specialization, tool governance and ruthless observability. Or watch your AI workforce implode.

We choose control. Don’t be greedy.

Disclaimer: This article is for informational purposes only and does not constitute professional advice. Organizations should consult with legal and technical experts before implementing AI systems. Trevolution Group makes no warranties about the completeness, reliability, or accuracy of this information.

