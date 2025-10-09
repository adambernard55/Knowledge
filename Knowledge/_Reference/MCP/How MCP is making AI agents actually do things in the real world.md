

![Raul Leite](https://www.infoworld.com/wp-content/uploads/2025/09/11702-0-01730300-1759136590-author_photo_Raul-Leite_1757530044.jpg?quality=50&strip=all&w=150)

by [Raul Leite](https://www.infoworld.com/profile/raul-leite/)



# How MCP is making AI agents actually do things in the real world

feature

Sep 29, 20257 mins

APIs[Development Tools](https://www.infoworld.com/development-tools/)[Generative AI](https://www.infoworld.com/generative-ai/)

## If you think AI is just talk, think again — MCP is turning chatbots into doers, and the future of work may never look the same.

![Ai technology. Artificial Intelligence. Humanoid Robot use AI tools for generate images, write code, writer bot, translate, advertising. AI assistant. Text to music, video. chatbot. LLM. AI robot.](https://www.infoworld.com/wp-content/uploads/2025/09/4064169-0-98060600-1759136604-shutterstock_2497349479_447e04.jpg?quality=50&strip=all&w=1024)

Credit: [BOY ANTHONY / Shutterstock](https://enterprise.shutterstock.com/image-photo/ai-technology-artificial-intelligence-humanoid-robot-2497349479)

You’ve seen them: Those incredible [large language models](https://www.infoworld.com/article/2335213/large-language-models-the-foundations-of-generative-ai.html) (LLMs) that can chat, write and even generate code. They’ve revolutionized how we interact with technology, but there’s a new, even more exciting chapter unfolding: the rise of [AI agents](https://www.infoworld.com/article/3611465/how-ai-agents-will-transform-the-future-of-work.html) that don’t just talk about tasks, they actually perform them in the real world. Think beyond a clever chatbot to an AI that can genuinely manage your calendar, analyze a report and then send a perfectly drafted email, all on its own.

The evolution from statically prompted LLMs to dynamic, tool-using AI agents represents a fundamental architectural shift in AI application design. This transition from generative to agentic systems is enabled by a critical new standard: the [Model Context Protocol](https://www.infoworld.com/article/4029634/what-is-model-context-protocol-how-mcp-bridges-ai-and-external-services.html) (MCP). For engineers and architects building at the forefront of AI, understanding MCP is not optional; it’s essential for constructing the interoperable and powerful AI agents that will define the next paradigm of human-computer interaction.

This protocol standardizes the interface between LLMs and external systems, moving beyond simple API wrappers to a formalized client-server model for tool, data and service integration.

This revolutionary protocol standardizes how AI models exchange information and interact with applications. It allows an AI agent to interpret broad instructions like “summarize this report and send an email” and then break them down into specific tool calls using MCP. This shift from passive generation to active execution is foundational to the agentic AI era. However, while MCP promises incredible benefits, it also introduces a whole new realm of cybersecurity vulnerabilities that demand our immediate and strategic attention.

## What is MCP, anyway?

MCP is not merely an API specification; it is a communication protocol that defines a clear contract between AI applications and the resources they consume. It employs a JSON-RPC 2.0 transport layer over either SSE (Server-Sent Events) or standard Unix sockets, facilitating a persistent, bidirectional connection for high-frequency tool invocation. Imagine if every device you owned needed a different charging cable. Frustrating, right? Now imagine a universal connector that lets everything communicate seamlessly. That’s essentially what MCP is for AI applications. It’s a universal standard that enables AI agents to seamlessly connect with external tools, data sources and services, dramatically boosting their functionality. I sometimes refer to it as the USB-C for AI.

In simpler terms, MCP defines how AI models exchange information and interact with various applications and systems. It uses a client-server architecture where an AI application (the ‘host’) sends requests to ‘servers’ through an ‘MCP client.’ These servers are the powerhouses, offering three core capabilities:

### 1.  MCP client

Integrated within the AI application or framework (e.g., an agent orchestration runtime), the client manages the session, transmits LLM-generated requests and handles server responses.

### 2.  MCP server

A standalone process that exposes a suite of capabilities to the client. Servers can be written in any language and are responsible for the actual execution of operations and data retrieval.

### 3.  Capabilities

The core functionalities a server exposes, defined in a structured schema:

- **Tools.** Callable functions that execute side effects or retrieve live data (e.g., ‘execute_sql_query’, ‘send_slack_message’, ‘create_jira_ticket’). Tools are defined with a name, description and a JSON Schema for their parameters.
- **Resources.** Uniformly named pointers to data streams (e.g., ‘file:///reports/q3.md’, ‘db://prod/users/table’). The server handles authentication, data retrieval and formatting, returning content as text or URI lists.
- **Prompts.** Pre-defined, parameterized prompt templates stored on the server, enabling consistent and optimized LLM instructions across different clients.

This decoupled architecture allows for a clean separation of concerns: The LLM-based agent handles planning and natural language understanding, while dedicated servers manage secure, governed access to tools and data sources.

## InfoWorld Smart Answers

 [Learn more](https://www.infoworld.com/smart-answers)

#### Explore related questions

- [How can Model Context Protocol (MCP) help CIOs avoid AI vendor lock-in?](https://www.infoworld.com/smart-answers?q=How%20can%20Model%20Context%20Protocol%20\(MCP\)%20help%20CIOs%20avoid%20AI%20vendor%20lock-in%3F&qs=article_infoworld_4064169)
- [What are Gartner's predicted business impacts of agentic AI by 2029?](https://www.infoworld.com/smart-answers?q=What%20are%20Gartner%27s%20predicted%20business%20impacts%20of%20agentic%20AI%20by%202029%3F&qs=article_infoworld_4064169)
- [What are specific examples of Anthropic's MCP server implementations?](https://www.infoworld.com/smart-answers?q=What%20are%20specific%20examples%20of%20Anthropic%27s%20MCP%20server%20implementations%3F&qs=article_infoworld_4064169)
- [What are specific cybersecurity vulnerabilities introduced by the Model Context Protocol (MCP)?](https://www.infoworld.com/smart-answers?q=What%20are%20specific%20cybersecurity%20vulnerabilities%20introduced%20by%20the%20Model%20Context%20Protocol%20\(MCP\)%3F&qs=article_infoworld_4064169)
- [Which company recently introduced the Model Context Protocol (MCP) standard?](https://www.infoworld.com/smart-answers?q=Which%20company%20recently%20introduced%20the%20Model%20Context%20Protocol%20\(MCP\)%20standard%3F&qs=article_infoworld_4064169)

Ask

Thanks to MCP, an AI agent can perform tasks like reading local files, querying databases or accessing networks, then return the results for further processing. It’s forming the backbone of modern AI agent ecosystems, supporting various services.

## Why MCP matters for the future of AI and how we use it

MCP is not just another technical specification; it’s a foundational building block for the next generation of AI, propelling us into the agentic AI era. Here’s why it’s such a big deal:

### From passive generation to active execution

Traditionally, LLMs were about generating content. MCP changes the game by enabling AI agents to interpret broad, human-like instructions (like “summarize this report and send an email to the right people”) and then break them down into specific tool calls. This allows AI to perform real-time tasks and actively engage with the world, moving beyond just talking to doing.

### Unlocking powerful, complex workflows

With MCP, AI agents can leverage a vast array of external tools. This means AI applications can incorporate external data and services without needing to manage the complexity of each API. This facilitates building incredibly complex and intelligent workflows, making AI agents much more useful and versatile in enterprise operations.

### Democratizing AI for everyone

MCP significantly lowers the barrier to integrating AI into everyday tasks. Anyone can now build and run an MCP server, often with just a few prompts. This accessibility means that non-experts can connect AI to email, project management or business systems in minutes, enabling entirely new categories of AI-driven orchestration and automation across organizations. Just as spreadsheets made data modeling accessible, MCP is doing the same for AI-driven workflows.

### Ensuring security, governance and compliance at scale

For enterprises, MCP servers introduce critical control points for data governance and privacy. They can centralize access to sensitive data, managing who can access what, performing dynamic data masking and ensuring only necessary and permitted data is accessed. This capability is vital for enforcing data privacy and compliance policies, reducing the risk of sensitive information leaking into AI models. It’s a strategic layer for scaling AI safely within the enterprise.

The rapid adoption of MCP — its core specification came together in just over a week, and within eight months, there were thousands of public servers — highlighting its immense value. This fast pace means that security must keep pace with innovation. While MCP offers incredible benefits, its relatively concise design also introduces significant security vulnerabilities. The very act of broadening the AI agent’s ability to interact with external tools expands the attack surface. Addressing these security challenges is not an afterthought, but a core component of successful AI adoption

## The future is agentic

The model context protocol is a transformative technology that is defining how AI systems connect to tools, data and each other. It’s the infrastructure that makes AI agents truly “agentic” — capable of understanding intent and taking action. Understanding MCP is key to grasping how AI will evolve from intelligent assistants to powerful, autonomous partners, fundamentally changing how we work, innovate and interact with the digital world. The future of AI is here, and it’s deeply intertwined with the secure evolution of MCP.

**This article is published as part of the Foundry Expert Contributor Network.**[**Want to join?**](https://www.infoworld.com/expert-contributor-network/)

![Raul Leite](https://www.infoworld.com/wp-content/uploads/2025/09/11702-0-01730300-1759136590-author_photo_Raul-Leite_1757530044.jpg?quality=50&strip=all&w=250)

by [Raul Leite](https://www.infoworld.com/profile/raul-leite/)

Contributor

1. [Follow Raul Leite on LinkedIn](https://www.linkedin.com/in/raul-leite/)

At Red Hat, [Raul Leite](https://www.linkedin.com/in/raul-leite/) serves as a principal solution architect, part of the largest open source software company in the world. Raul is a seasoned problem solver, architect and engineer with experience spanning the public sector, global enterprises, and diverse markets. His career has taken him from Brazil across Latin America and now into the USA, giving him a unique perspective on how open source technologies are developed, delivered and maintained to drive customer innovation worldwide.

## More from this author

- [
    
    feature
    
    ### Unlocking LLM superpowers: How PagedAttention helps the memory maze
    
    Sep 11, 20255 mins
    
    ](https://www.infoworld.com/article/4055048/unlocking-llm-superpowers-how-pagedattention-helps-the-memory-maze.html)

## Show me more

PopularArticlesVideos

[

news

### IBM launches Granite 4.0 to cut AI infra costs with hybrid Mamba-transformer models

By Gyana Swain

Oct 3, 20256 mins

Artificial Intelligence

![Image](https://www.infoworld.com/wp-content/uploads/2025/10/4067691-0-08268700-1759491096-shutterstock_2284126663-100943536-orig.jpg?quality=50&strip=all&w=375)

](https://www.infoworld.com/article/4067691/ibm-launches-granite-4-0-to-cut-ai-infra-costs-with-hybrid-mamba-transformer-models.html)

[

video

### Make Python apps redistributable with PyCrucible

Oct 1, 20253 mins

Python

![Image](https://www.infoworld.com/wp-content/uploads/2025/10/4066576-0-32066100-1759346227-youtube-thumbnail-3xRsxhtzfZg_9e8f98.jpg?quality=50&strip=all&w=444)

](https://www.infoworld.com/video/4066576/make-python-apps-redistributable-with-pycrucible.html)

[

video

### Python 3.14's live debugging interface

Sep 23, 20254 mins

Python

![Image](https://www.infoworld.com/wp-content/uploads/2025/09/4061721-0-37580600-1758640624-youtube-thumbnail-hJXbUPIZ-nc_41599f.jpg?quality=50&strip=all&w=444)

](https://www.infoworld.com/video/4061721/python-3-14s-live-debugging-interface.html)

Sponsored Links

- [Secure AI by Design: Unleash the power of AI and keep applications, usage and data secure.](http://pubads.g.doubleclick.net/gampad/clk?id=6856108221&iu=/8456/IDG.G_B2B_InfoWorld.com)
- [Solve your most complex IT challenges with solutions that simplify your modernization journey.](http://pubads.g.doubleclick.net/gampad/clk?id=7038222634&iu=/8456/IDG.G_B2B_InfoWorld.com)
- [Empower your cybersecurity team with expert insights from Palo Alto Networks.](http://pubads.g.doubleclick.net/gampad/clk?id=6858508033&iu=/8456/IDG.G_B2B_InfoWorld.com)

About 

- [About Us](https://www.infoworld.com/about-us/)
- [Advertise](https://foundryco.com/our-brands/infoworld/)
- [Contact Us](https://www.infoworld.com/contact-us/)
- [Editorial Ethics Policy](https://www.infoworld.com/editorial-ethics-policy/)
- [Foundry Careers](https://foundryco.com/work-here/)
- [Reprints](https://www.infoworld.com/contact-us/#republication-permissions)
- [Newsletters](https://www.infoworld.com/newsletters/signup/)
- [BrandPosts](https://www.infoworld.com/brandposts/)

Policies 

- [Terms of Service](https://foundryco.com/terms-of-service-agreement/)
- [Privacy Policy](https://foundryco.com/privacy-policy/)
- [Cookie Policy](https://foundryco.com/cookie-policy/)
- [Copyright Notice](https://foundryco.com/copyright-notice/)
- [Member Preferences](https://www.infoworld.com/member-preferences/)
- [About AdChoices](https://foundryco.com/ad-choices/)
- [Your California Privacy Rights](https://foundryco.com/ccpa/)
- Do Not Sell or Share My Personal Information

Our Network 

- [CIO](https://www.cio.com/)
- [Computerworld](https://www.computerworld.com/)
- [CSO](https://www.csoonline.com/)
- [Network World](https://www.networkworld.com/)

[](https://foundryco.com/)

- [](https://www.facebook.com/InfoWorld)
- [](https://twitter.com/infoworld)
- [](https://www.youtube.com/@InfoWorld)
- [](https://news.google.com/publications/CAAqIggKIhxDQkFTRHdvSkwyMHZNRFY1ZEhaNUVnSmxiaWdBUAE)
- [](https://www.linkedin.com/company/164364)

[© 2025 FoundryCo, Inc. All Rights Reserved.](https://foundryco.com/terms-of-service-agreement/)

![](https://sync.intentiq.com/profiles_engine/ProfilesEngineServlet?at=20&mi=10&secure=1&dpi=456059511&iiqidtype=2&iiqpcid=7ac908d8-ceaf-48e7-8ab1-5af351098832&iiqpciddate=1750464839020&tsrnd=847_1759577290510&vrref=android-app%3A%2F%2Fcom.google.android.googlequicksearchbox%2F&jsver=5.082)