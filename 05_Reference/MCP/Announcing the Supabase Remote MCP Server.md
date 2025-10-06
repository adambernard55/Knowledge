
03 Oct 2025

Today we are launching our remote MCP server, allowing you to connect your Supabase projects with many more AI agents than before, including ChatGPT, Claude, and Builder.io. We also added support for MCP auth (OAuth2), a faster and more secure way to connect agents with your Supabase account (via browser-based authentication). Last but not least, we’re adding official MCP support for local Supabase instances created through the CLI.

Now all you need is a single URL to connect your favorite AI agent to Supabase:

https://mcp.supabase.com/mcp

Or if you’re running Supabase locally:

http://localhost:54321/mcp

On top of this, we're adding even more tools to Supabase MCP that we hope will make you more productive.

What is MCP?#
MCP stands for Model Context Protocol. It standardizes how Large Language Models (LLMs) talk to platforms like Supabase. We released the initial version of our Supabase MCP server back in April which allowed folks to connect their favorite AI tools (like Cursor or Claude Code) directly with Supabase. Since launch, we've added many new features:

Fetching and deploying Edge Functions
Fetching logs from any service in the Supabase stack
Scoping the server to only one project
Putting the server in read-only mode (implemented at the Postgres role level)
Today we are adding even more tools to the tool belt, along with a new way to connect: Remote HTTP.

What is remote MCP?#
The MCP protocol supports 2 official methods for running servers, known as transports:

stdio: The MCP server runs directly on your local machine (using a runtime like npx) and communicates over a standard I/O interface. This was the transport used by most MCP servers originally, including Supabase.
Remote (HTTP): The MCP server runs remotely over an HTTP web server.
When we first launched the Supabase MCP server, the HTTP transport was going through some spec changes which many clients didn't support yet (streamable HTTP). We wanted to support all clients immediately without investing a ton of time into an unstable transport, so we released our server as an stdio MCP server. To run this, you needed to install Node.js and configuring your MCP client with this npx command:

npx -y @supabase/mcp-server-supabase@latest ...

This worked great at the time, because it meant that folks could connect their AI agents with Supabase immediately without a lot of infrastructure work on our end. But it came with some downsides:

Limited MCP clients: Most web-based AI agents (like ChatGPT, Claude.ai, Builder.io) are limited to HTTP-based MCP servers due to the environments they run in. APIs like OpenAI's Response API also have support for MCP - but only with remote MCP servers, not stdio.
Authentication was clunky: Folks needed to manually generate a personal access token (PAT) in order to authenticate with their Supabase account which added friction to the setup experience and not very secure (it was easy to accidentally commit this to source control).
Errors were hard to debug: Not only did everyone need Node.js installed to run the server, they also needed to make sure their environment was configured correctly so that the correct version was used by each client (if you used nvm). Every operating system also had slight differences that required modifying the npx command accordingly.
On the other hand, Remote MCP only requires a single URL:

https://mcp.supabase.com/mcp

# or http://localhost:54321/mcp for local

We built this interactive widget to help you connect popular MCP clients to Supabase and customize the URL to your preferences (like project-scoped mode and read-only mode):

New features#
Our philosophy on Supabase MCP comes down to two ideas:

Supabase MCP should be used for development. It was designed from the beginning to assist with app development and shouldn’t be connected to production databases. See our post on Defense in Depth for MCP Servers.
MCP is just another way to access the same platform features that you already use in the web dashboard and CLI. But - since it’s being used by an LLM, it should also lean into the strengths of an AI-first UX.
With these in mind, we added the following new features to assist AI agents while they help build your app:

Feature groups
Doc search
Security and performance advisors
Storage
Feature groups#
Feature groups allow you to pick and choose which tools you want to expose to your agent. This is useful in two ways:

Security: if you know that you never want your agent to deploy edge functions, you can remove the functions feature group so that those tools are never exposed to the LLM.
Tool limits: many MCP clients limit the number of tools you can connect to them, so feature groups allow you to only connect the tools that you use regularly.
Today we support the following feature groups:

account
docs
database
debugging
development
functions
storage
branching
See the MCP docs for instructions on how to enable or disable these groups.

Doc search#
A big challenge with LLMs is knowledge cutoff. Early LLMs had a pretty good understanding of Supabase, but it was still nowhere near complete or up-to-date. Now the latest leading LLMs have a very good understanding of Supabase, but will still lag behind any new features, bug fixes, or other updates that we make.

To help with this, we added a new tool to Supabase MCP: search_docs. This tool exposes the latest up-to-date Supabase docs powered by our Content API - a GraphQL based search API that uses hybrid search (semantic + keyword) to find relevant documentation for a given query (formatted as markdown).

The result ends up looking like similar to self-RAG: whenever your agent needs clarification on a topic, it can use this tool to find the most relevant Supabase documentation.

Security and performance advisors#
How do you know if LLMs are really following best coding practices? When a project reaches even a moderate level of complexity, the amount of context and moving parts becomes a real challenge to navigate, even for humans.

Our solution is a feature that already exists on our platform - advisors. Advisors are essentially lints on your database that help you follow security and performance best practices. We added a new get_advisors MCP tool that fetches these same advisors so that your agent can both discover and fix any outstanding issues on your database.

Storage#
We also added initial support for Supabase Storage on our MCP server. This first version allows your agent to see which buckets exist on your project and update their configuration, but in the future we’ll look into more abilities like listing files and their details.

This feature was actually a community contribution (thanks Nico!). If there are ever missing features that you’d like to see, PR’s are always welcome!

What’s next?#
We have more exciting plans for MCP at Supabase:

Security: Today our OAuth2 implementation requires you to make a binary decision on permissions: either grant all permissions to your MCP client, or none. This isn’t ideal if you know that you never want to, say, allow your client to access to your Edge Functions.
To improve this, we’re working to support fine-grain permissions that can be toggled during authorization. It’s a big task to re-work our permission infrastructure to support this, but we believe it’s worth it. 2. Double down on local: We’re very excited to support local Supabase instances in this release, but we also believe there is a lot more that can be done. Supabase MCP is designed to be used for development, so we want the local experience to be first-class. 3. Build your own MCP: You might have thought about building your own MCP server on top of Supabase. We’re using the playbook and lessons learned from our own MCP server to provide the tools you need to do the same - including remote MCP and auth. Stay tuned!

We’re keen to continue investing in MCP and excited to see how you use these new features!

Share this article
Last post

gm 👋 web3, welcome aboard to Sign in with Web3 (Solana, Ethereum)
3 October 2025

Next post

Enterprise speed, enterprise standards with Bolt Cloud + Supabase
30 September 2025
