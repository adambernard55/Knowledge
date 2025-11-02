Sep 30, 2025 2 min read

by

Author photo
Robert Krzaczyński

Senior Software Engineer
Log in to listen to this article
Perplexity has introduced the Search API, opening up access to the same infrastructure that underpins its public answer engine. With coverage of hundreds of billions of webpages and infrastructure tuned for AI-heavy workloads, the new API is aimed at developers who want real-time, reliable search results for building their own agents, applications, and retrieval-augmented pipelines.

For years, large-scale search has been dominated by providers with closed indices. Perplexity takes a different approach: rather than returning full documents, its API delivers fine-grained, pre-ranked snippets. This reduces the need for additional preprocessing and enables faster integration into AI-driven workflows.

The company stresses that accuracy and freshness are central. Its indexing system updates tens of thousands of documents per second, supported by an AI-powered content understanding module that parses messy web data in real time. This means results are not only broad in coverage but also timely—a key need for AI systems that risk hallucination or outdated answers when grounded on stale data.

On Reddit, some developers immediately questioned how the new release fits alongside Perplexity’s existing tools:

How is this different from the current API?

One Perplexity’s developer answered:

Unlike the current Sonar API, which returns synthesized answers, the new Search API gives raw, ranked web results—so you can use them directly, ground other models, or build your own agents on top! 

Others highlighted the speed improvements. Abhilash Jaiswal, an AI expert at Atos, noted:

This speed is amazing, so kudos to the team who worked on it. Yes, after Bing's retirement, there was a situation to find the alternatives quickly, so it got into a bit of instability. However, we found the sort of solution for our needs. To leverage this, we need to have it in the developer community, preferably as MCP.

To back up its claims, Perplexity is releasing search_evals, an open-source evaluation framework for testing different search APIs. In early results, the Search API is reported to outperform competitors on both single-step queries and multi-step agentic research workflows, scoring higher on both quality and latency. Perplexity also says that, thanks to infrastructure efficiencies, it can deliver these gains at lower cost.

Ease of use has also been emphasized. Alongside the API, Perplexity is shipping a developer console, documentation, and a Search SDK, which the company’s own engineers have used to prototype new features in under an hour. The API is available now via the Perplexity API Platform, which also hosts the Sonar API.

With real-time freshness, structured responses, and raw web access, the Perplexity Search API could become an essential building block for the next wave of AI agents and retrieval-heavy products.

About the Author

Robert Krzaczyński
Robert Krzaczyński is a software engineer with solid experience in developing web applications. Passionate about applying artificial intelligence algorithms in medicine and the broader healthcare sector, he continuously expands his expertise in ML and AI. He holds a BSc Eng degree in Control Engineering and Robotics, as well as an MSc Eng degree in Computer Science.

Show more
This content is in the AI, ML & Data Engineering topic
Related Topics: 

 

 

 

Popular in AI, ML & Data Engineering
OpenAI's GPT-5 Now Generally Available on Microsoft Azure AI Foundry
Replit Introduces Agent 3 for Extended Autonomous Coding and Automation
OpenAI Releases GPT-5-Codex Optimized for Complex Code Refactoring and Code Reviews
Google's Agent Development Kit for Java Adds Integration with LangChain4j
InfoQ AI, ML and Data Engineering Trends Report - 2025
Related Sponsors
Principles and Patterns for Distributed Application Architecture (By O'Reilly) - Download Now
Enterprise Agentic Architecture: Performance, Cost, and Safety in Production (Live Webinar Oct 9, 2025) - Save Your Seat
From BPMN to Agentic AI: Building the Future of Intelligent Automation
Designing Data Intensive Applications (By O'Reilly)
Akka Launches Agentic Platform for Autonomous, Real-Time & Edge AI Systems
Related Sponsor
Related sponsor icon
Build and run data, API, and agentic AI services on the world's most widely adopted actor-based runtime. Akka services are elastic, agile, and resilient. Learn more.

Development
A Thirteen Billion Year Old Photograph
Cursor 1.7 Adds Hooks for Agent Lifecycle Control
Myth Busters: is Rust a Slam Dunk?
Architecture & Design
Agoda Leverages ChatGPT in the CI/CD Process for SQL Stored Procedure Optimization
Scaling the BBC Design System: Tooling, Community, Governance and Gardening
A Pipeline Approach to Language Migrations
Culture & Methods
Building Engineering Culture Through Autonomy and Ownership
How Software Engineers Can Grow into Staff Plus Roles
Your Roadmap to a Fulfilling Career: The Pillars of Staff+ Growth
AI, ML & Data Engineering
Anthropic Reveals Three Infrastructure Bugs Behind Claude Performance Issues
Beyond the Hype: Architecting Systems with Agentic AI
Microsoft Tests Microfluidic Cooling for Next-Generation AI Chips
DevOps
Microsoft Announces General Availability of AKS Automatic
Pulumi Launches Neo: an Agentic AI Platform Engineer for Multi-Cloud Infrastructure
DORA Report Finds AI Is an Amplifier in Software Development, But Trust Remains Low
The InfoQ Newsletter
A round-up of last week’s content on InfoQ sent out every Tuesday. Join a community of over 250,000 senior developers. View an example

Get a quick overview of content published on a variety of innovator and early adopter technologies
Learn what you don’t know that you don’t know
Stay up to date with the latest information from the topics you are interested in
Enter your e-mail address
We protect your privacy.

October 15-16, 2025 | Munich, Germany

Get proven, real-world solutions directly from senior engineering leaders navigating Europe's toughest technical challenges. Learn directly from leaders at:

Mercedes-Benz Tech Motion: Lessons from building a global content delivery network.
Allianz Direct: Navigating a four-year journey from legacy monolith to sovereignty.
SoundCloud: Architecting for performance and scale on a global platform.
Zalando: Driving real platform engagement with Graph Neural Networks

Last chance to register!
Home
Create account
QCon Conferences
Events
Write for InfoQ
InfoQ Editors
About InfoQ
About C4Media
Media Kit
InfoQ Developer Marketing Blog
Diversity
Events
InfoQ Dev Summit Munich
October 15-16, 2025
QCon San Francisco
November 17-21, 2025 / In-person
QCon AI New York
December 16-17, 2025
QCon London
March 16-19, 2026
Follow us on
Youtube
232K Followers
Linkedin
26K Followers
RSS
19K Readers
X
57.1k Followers
Facebook
21K Likes
Bluesky
New
Alexa
New
Stay in the know
The InfoQ Podcast
The InfoQ Podcast Logo - Stay in the know
Engineering Culture Podcast
Engineering Culture Podcast Logo - Stay in the knoww
The Software Architects' Newsletter
The Software Architects' Newsletter Logo - Stay in the know
General Feedback
feedback@infoq.com
Advertising
sales@infoq.com
Editorial
editors@infoq.com
Marketing
marketing@infoq.com
InfoQ.com and all content copyright © 2006-2025 C4Media Inc.
Privacy Notice, Terms And Conditions, Cookie Policy

