
Mapping Power, Concentration, and Usage in the Emerging AI Developer Ecosystem


By [Ilan Strauss](https://www.oreilly.com/people/ilan-strauss/), [Sruly Rosenblat](https://www.oreilly.com/people/sruly-rosenblat/), [Isobel Moure](https://www.oreilly.com/people/isobel-moure/) and [Tim O’Reilly](https://www.oreilly.com/people/tim-oreilly/)September 16, 2025 • 14 minute read

Share

[](https://www.linkedin.com/feed/?shareActive&mini=true&text=O%E2%80%99Reilly%20-%20MCP%20in%20Practice%20with%20Ilan%20Strauss%2C%20Sruly%20Rosenblat%2C%20Isobel%20Moure%2C%20and%20Tim%20O%E2%80%99Reilly%20-%20https%3A%2F%2Fwww.oreilly.com%2Fradar%2Fmcp-in-practice%2F)[](https://x.com/intent/tweet?original_referer=https%3A%2F%2Fwww.oreilly.com%2F&text=O%E2%80%99Reilly%20-%20MCP%20in%20Practice%20with%20Ilan%20Strauss%2C%20Sruly%20Rosenblat%2C%20Isobel%20Moure%2C%20and%20Tim%20O%E2%80%99Reilly%20-%20https%3A%2F%2Fwww.oreilly.com%2Fradar%2Fmcp-in-practice%2F)[](https://www.facebook.com/sharer/sharer.php?quote=O%E2%80%99Reilly%20-%20MCP%20in%20Practice%20with%20Ilan%20Strauss%2C%20Sruly%20Rosenblat%2C%20Isobel%20Moure%2C%20and%20Tim%20O%E2%80%99Reilly%20-%20https%3A%2F%2Fwww.oreilly.com%2Fradar%2Fmcp-in-practice%2F)[](https://threads.net/intent/post?text=O%E2%80%99Reilly%20-%20MCP%20in%20Practice%20with%20Ilan%20Strauss%2C%20Sruly%20Rosenblat%2C%20Isobel%20Moure%2C%20and%20Tim%20O%E2%80%99Reilly%20-%20https%3A%2F%2Fwww.oreilly.com%2Fradar%2Fmcp-in-practice%2F)[](https://bsky.app/intent/compose?text=O%E2%80%99Reilly%20-%20MCP%20in%20Practice%20with%20Ilan%20Strauss%2C%20Sruly%20Rosenblat%2C%20Isobel%20Moure%2C%20and%20Tim%20O%E2%80%99Reilly%20-%20https%3A%2F%2Fwww.oreilly.com%2Fradar%2Fmcp-in-practice%2F)

_The following was [originally published in](https://asimovaddendum.substack.com/p/read-write-act-inside-the-mcp-server)_ [Asimov’s Addendum](https://asimovaddendum.substack.com/p/read-write-act-inside-the-mcp-server)_,_ _September 11, 2025._  
  
_Learn more about the AI Disclosures Project [here](https://www.ssrc.org/programs/ai-disclosures-project/)._

## **1. The Rise and Rise of MCP**

Anthropic’s [Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) (MCP) was released in November 2024 as a way to make tools and platforms model-agnostic. MCP works by defining servers and clients. MCP servers are local or remote end points where tools and resources are defined. For example, GitHub released an MCP server that allows LLMs to both read from and write to GitHub. MCP clients are the connection from an AI application to MCP servers—they allow an LLM to interact with context and tools from different servers. An example of an MCP client is Claude Desktop, which allows the Claude models to interact with thousands of MCP servers.

**In a relatively short time, MCP has become the backbone of hundreds of AI pipelines and applications**. Major players like Anthropic and OpenAI have built it into their products. Developer tools such as Cursor (a coding-focused text editor or IDE) and productivity apps like [Raycast](https://www.raycast.com/EvanZhouDev/mcp) also use MCP. Additionally, thousands of [developers](https://arxiv.org/abs/2506.13538) use it to integrate AI models and access external tools and data without having to build an entire ecosystem from scratch.

In previous work published with _AI Frontiers_, we argued that [MCP can act](https://ai-frontiers.org/articles/open-protocols-prevent-ai-monopolies) as a great unbundler of “context”—the data that helps AI applications provide more relevant answers to consumers. In doing so, it can help decentralize AI markets. **We argued that, for MCP to truly achieve its goals, it requires support from**:

1. **Open APIs**: So that MCP applications can access third-party tools for agentic use (_write_ actions) and context (_read_)
2. **Fluid memory**: Interoperable LLM memory standards, accessed via MCP-like open protocols, so that the memory context accrued at OpenAI and other leading developers does not get stuck there, preventing downstream innovation

We expand upon these two points in a [recent policy note](https://ssrc-static.s3.us-east-1.amazonaws.com/Protocols-and-Power-Moure-OReilly-Strauss_SSRC_08272025.pdf), for those looking to dig deeper.

More generally, **we argue that protocols**, **like MCP**, **are actually** [**foundational “rules of the road” for AI markets**](https://asimovaddendum.substack.com/p/disclosures-i-do-not-think-that-word), _whereby open disclosure and communication standards are built_ _into the network itself_, rather than imposed _after the fact_ by regulators. Protocols are fundamentally market-shaping devices, architecting markets through the permissions, rules, and interoperability of the network itself. They can have a big impact on how the commercial markets built on top of them function too.

### **1.1 But how is the MCP ecosystem evolving?**

**Yet we don’t have a clear idea of the shape of the MCP ecosystem today**. _What are the most common use cases of MCP? What sort of access is being given by MCP servers and used by MCP clients? Is the data accessed via MCP “read-only” for context, or does it allow agents to “write” and interact with it—for example, by editing files or sending emails?_

To begin answering these questions, we look at the tools and context which AI agents use via _MCP servers_. This gives us a clue about what is being built and what is getting attention. In this article, we don’t analyze _MCP clients_—the applications that use MCP servers. We instead limit our analysis to what MCP servers are making available for building.

We assembled a large dataset of MCP servers (n = 2,874), scraped from [Pulse](https://www.pulsemcp.com/).1 We then enriched it with GitHub star-count data on each server. On GitHub, stars are similar to Facebook “likes,” and [developers use them](https://homepages.dcc.ufmg.br/~mtov/pub/2018-jss-github-stars.pdf) to show appreciation, bookmark projects, or indicate usage.

In practice, _while there were plenty of MCP servers, we found that the top few garnered most of the attention and, likely by extension, most of the use._ **Just the top 10 servers had nearly half of all GitHub stars given to MCP servers**.

**Some of our takeaways are:**

1. _MCP usage appears to be fairly concentrated_. This means that, if left unchecked, a small number of servers and (by extension) APIs could have outsize control over the MCP ecosystem being created.
2. _MCP use (tools and data being accessed) is dominated by just three categories_: Database & Search (RAG), Computer & Web Automation, and Software Engineering. Together, they received nearly three-quarters (72.6%) of all stars on GitHub (which we proxy for usage).
3. Most MCP servers support both _read_ (access context) and _write_ (change context) operations, showing that developers want their agents to be able to act on context, not just consume it.

## **2. Findings**

_To start with, we analyzed the MCP ecosystem for concentration risk._

### **2.1 MCP server use is concentrated**

**We found that MCP usage is concentrated among several key MCP servers**, _judged by the number of GitHub stars each repo received_.

Despite there being thousands of MCP servers, **the top 10 servers make up nearly half (45.7%) of all GitHub stars given to MCP servers** (pie chart below) and the top 10% of servers make up 88.3% of all GitHub stars (not shown).

![The top 10 servers received 45.7% of all GitHub stars in our dataset of 2,874 servers.](https://www.oreilly.com/radar/wp-content/uploads/sites/3/2025/09/image-2.png "Chart")

_The top 10 servers received 45.7% of all GitHub stars in our dataset of_ 2,874 _servers._

_This means that the majority of real-world MCP users are likely relying on the same few services made available via a handful of APIs_. This concentration likely stems from network effects and practical utility: All developers gravitate toward servers that solve universal problems like web browsing, database access, and integration with widely used platforms like GitHub, Figma, and Blender. This concentration pattern seems typical of developer-tool ecosystems. A few well-executed, broadly applicable solutions tend to dominate. Meanwhile, more specialized tools occupy smaller niches.

### **2.2 The top 10 MCP servers really matter**

Next, the top 10 MCP servers are shown in the table below, along with their star count and what they do.

**Among the top 10 MCP servers,** _GitHub,_ _Repomix_, _Context7_, and _Framelink_ are built to assist with software development: _Context7_ and _Repomix_ by gathering context, _GitHub_ by allowing agents to interact with projects, and _Framelink_ by passing on the design specifications from _Figma_ directly to the model. The _Blender_ server allows agents to create 3D models of anything, using the popular open source _Blender_ application. Finally, _Activepieces_ and _MindsDB_ connect the agent to multiple APIs with one standardized interface: in _MindsDB_’s case, primarily to read data from databases, and in _Activepieces_ to automate services.

![The top 10 MCP servers with short descriptions, design courtesy of Claude.](https://www.oreilly.com/radar/wp-content/uploads/sites/3/2025/09/image-3.png)

_The top 10 MCP servers with short descriptions, design courtesy of Claude._

**The dominance of agentic browsing**, **in the form of _Browser Use_ (61,000 stars) and _Playwright MCP_ (18,425 stars)**, **stands out**. This reflects the fundamental need for AI systems to interact with web content. These tools allow AI to navigate websites, click buttons, fill out forms, and extract data just like a human would. _Agentic browsing has surged_, _even though it’s far less token-efficient than calling an API_. Browsing agents often need to wade through multiple pages of boilerplate to extract slivers of data a single API request could return. Because many services lack usable APIs or tightly gate them, browser-based agents are often the simplest—sometimes the only—way to integrate, underscoring the limits of today’s APIs.

**Some of the top servers are unofficial.** Both the _Framelink_ and _Blender MCP_ are servers that interact with just a single application, but they are both “unofficial” products. This means that they are not officially endorsed by the developers of the application they are integrating with—those who own the underlying service or API (e.g., GitHub, Slack, Google). Instead, they are built by independent developers who create a bridge between an AI client and a service—often by reverse-engineering APIs, wrapping unofficial SDKs, or using browser automation to mimic user interactions.

It is healthy that third-party developers can build their own MCP servers, since this openness encourages innovation. But it also introduces an intermediary layer between the user and the API, which brings risks around trust, verification, and even potential abuse. With open source local servers, the code is transparent and can be vetted. By contrast, remote third-party servers are harder to audit, since users must trust code they can’t easily inspect.

**At a deeper level, the repos that currently dominate MCP servers highlight three encouraging facts about the MCP ecosystem:**

1. **First, several prominent MCP servers support multiple third-party services for their functionality.** _MindsDB_ and _Activepieces_ serve as gateways to multiple (often competing) service providers through a single server. _MindsDB_ allows developers to query different databases like PostgreSQL, MongoDB, and MySQL through a single interface, while _Taskmaster_ allows the agent to delegate tasks to a range of AI models from OpenAI, Anthropic, and Google, all without changing servers.
2. **Second, agentic browsing MCP servers are being used to get around potentially restrictive APIs.** As noted above, _Browser Use_ and _Playwright_ access internet services through a web browser, helping to bypass API restrictions, but they instead run up against anti-bot protections. This circumvents the limitations that APIs can impose on what developers are able to build.
3. **Third, some MCP servers do their processing on the developer’s computer (locally)**, **making them less dependent on a vendor maintaining API access**. _Some MCP servers examined here can run entirely on a local computer without sending data to the cloud—meaning that no gatekeeper has the power to cut you off_. Of the 10 MCP servers examined above, only _Framelink_, _Context7,_ and _GitHub_ rely on just a single cloud-only API dependency that can’t be run locally end-to-end on your machine. _Blender_ and _Repomix_ are completely open source and don’t require any internet access to work, while _MindsDB_, _Browser Use,_ and _Activepieces_ have local open source implementations.

### **2.3 The three categories that dominate MCP use**

_Next, we grouped MCP servers into different categories based on their functionality_.

When we analyzed what types of servers are most popular, we found that three dominated: **Computer & Web Automation (24.8%)**, **Software Engineering (24.7%)**, and **Database & Search (23.1%)**.

![Software engineering, computer and web automation, and database and search received 72.6% of all stars given to MCP servers.](https://www.oreilly.com/radar/wp-content/uploads/sites/3/2025/09/image-4.png "Chart")

_Software Engineering, Computer & Web Automation, and Database & Search received 72.6% of all stars given to MCP servers._

Widespread use of Software Engineering (24.7%) MCP servers aligns with [Anthropic’s economic index](https://arxiv.org/abs/2503.04761), which found that an outsize portion of AI interactions were related to software development.

The popularity of both Computer & Web Automation (24.8%) and Database & Search (23.1%) also makes sense. Before the advent of MCP, web scraping and database search were highly integrated applications across platforms like ChatGPT, Perplexity, and Gemini. With MCP, however, users can now access that same search functionality and connect their agents to any database with minimal effort. In other words, MCP’s [unbundling](https://ai-frontiers.org/articles/open-protocols-prevent-ai-monopolies) effect is highly visible here.

### **2.4 Agents interact with their environments**

_Lastly, we analyzed the capabilities of these servers_: Are they allowing AI applications just to access data and tools (_read_), or instead do agentic operations with them (_write_)?

**Across all but two of the MCP server categories looked at, the most popular MCP servers supported both _reading_ (access context) and _writing_ (agentic) operations**—shown in turquoise. The prevalence of servers with combined read and write access suggests that agents are not being built just to answer questions based on data but also to take action and interact with services on a user’s behalf.

![Showing MCP servers by category. Dotted red line at 10,000 stars (likes). The most popular servers support both read and write operations by agents. In contrast, almost no servers support just write operations.](https://www.oreilly.com/radar/wp-content/uploads/sites/3/2025/09/image-5.png)

_Showing MCP servers by category. Dotted red line at 10,000 stars (likes). The most popular servers support both read and write operations by agents. In contrast, almost no servers support just write operations._

The two exceptions are Database & Search (RAG) and Finance MCP servers, in which _read-only_ access is a common permission given. This is likely because data integrity is critical to ensuring reliability.

## **3. The Importance of Multiple Access Points**

A few implications of our analysis can be drawn out at this preliminary stage.

**First, concentrated MCP server use compounds the risks of API access being restricted**. As we discussed in “[Protocols and Power](https://asimovaddendum.substack.com/p/protocols-and-power),” MCP remains constrained by “_what a particular service (such as GitHub or Slack) happens to expose through its API_.” A few powerful digital service providers have the power to shut down access to their servers.

_One important hedge against API gatekeeping is that many of the top servers try not to rely on a single provide_r. **In addition**, **the following two safeguards are relevant**:

- **They offer local processing** of data on a user’s machine whenever possible, instead of sending the data for processing to a third-party server. Local processing ensures that functionality cannot be restricted.
- If running a service locally is not possible (e.g., email or web search), the server should still **support multiple avenues of getting at the needed context through competing APIs**. For example, _MindsDB_ functions as a gateway to multiple data sources, so instead of relying on just one database to read and write data, it goes to great lengths to support multiple databases in one unified interface, essentially making the backend tools interchangeable.

**Second, our analysis points to the fact that current restrictive API access policies are not sustainable.** Web scraping and bots, accessed via MCP servers, are probably being used (at least in part) to circumvent overly restrictive API access, complicating the [increasingly common](https://slate.com/technology/2025/08/uk-online-safety-act-reddit-wikipedia-open-internet.html) practice of banning bots. Even OpenAI is coloring outside the API lines, using a third-party service to access Google Search’s results through web scraping, thereby [circumventing its restrictive API](https://www.theinformation.com/articles/openai-challenging-google-using-search-data).

**Expanding structured API access in a meaningful way is vital**. _This ensures that legitimate AI automation runs through stable, documented end points._ Otherwise, developers resort to brittle browser automation where privacy and authorization have not been properly addressed. Regulatory guidance [could push](https://ai-frontiers.org/articles/open-protocols-prevent-ai-monopolies) the market in this direction, as with open banking in the US.

**Finally, encouraging greater transparency and disclosure** could help identify where the bottlenecks in the MCP ecosystem are.

- Developers operating popular MCP servers (above a certain usage threshold) or providing APIs used by top servers should report usage statistics, access denials, and rate-limiting policies. This data would help regulators identify emerging bottlenecks before they become entrenched. _GitHub might facilitate this by encouraging these disclosures, for example_.
- Additionally, MCP servers above certain usage thresholds should clearly list their dependencies on external APIs and what fallback options exist if the primary APIs become unavailable. This is not only helpful in determining the market structure, but also essential information for security and robustness for downstream applications.

The goal is not to eliminate all concentration in the network but to ensure that the MCP ecosystem remains contestable, with multiple viable paths for innovation and user choice. By addressing both technical architecture and market dynamics, these suggested tweaks could help MCP achieve its potential as a democratizing force in AI development, rather than merely shifting bottlenecks from one layer to another.

---

## Footnotes

1. For this analysis, we categorized each repo into one of 15 categories using GPT-5 mini. We then human-reviewed and edited the top 50 servers that make up around 70% of the total star count in our dataset.

---

## **Appendix**

### **Dataset**

The full dataset, along with descriptions of the categories, can be found here (constructed by Sruly Rosenblat):

[https://huggingface.co/datasets/sruly/MCP-In-Practice](https://huggingface.co/datasets/sruly/MCP-In-Practice)

### **Limitations**

There are a few limitations to our preliminary research:

- GitHub stars aren’t a measure of download counts or even necessarily a repo’s popularity.
- Only the name and description were used when categorizing repos with the LLM.
- Categorization was subject to both human and AI errors and many servers would likely fit into multiple categories.
- We only used the Pulse list for our dataset; other lists had different servers (e.g., Browser Use isn’t on mcpmarket.com).
- We excluded some repos from our analysis, such as those that had multiple servers and those we weren’t able to fetch the star count for. We may miss some popular servers by doing this.

### **MCP Server Use Over Time**

![The growth of the top nine repos’ star count over time from MCP’s launch date on November 25, 2024, until September 2025. NOTE: We were only able to track the Browser-Use’s repo until 40,000 stars; hence the flat line for its graph. In reality, roughly 21,000 stars were added over the next few months (the other graphs in this blog are properly adjusted).](https://www.oreilly.com/radar/wp-content/uploads/sites/3/2025/09/image-6.png)

_The growth of the top nine repos’ star count over time from MCP’s launch date on November 25, 2024, until September 2025._  
  
_Note: We were only able to track Browser Use’s repo until 40,000 stars; hence the flat line for its graph. In reality, roughly 21,000 stars were added over the next few months. (The other graphs in this post are properly adjusted.)_