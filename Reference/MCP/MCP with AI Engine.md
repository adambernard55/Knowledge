# MCP (Model Context Protocol)

An MCP server works as a structured API-like interface for AI models: the client (such as the Claude app) connects to the server, which exposes a set of endpoints declaring available tools and functions. This enables the model to perform actions like querying, updating, or managing your WordPress content.

You have two main approaches:

- **Using your WordPress as the MCP server**: Claude connects to your WordPress site and gains capabilities based on what is exposed through AI Engine.
- **Using Claude inside AI Engine**: Here, Claude becomes the internal model in AI Engine, and it can connect to a _remote_ MCP server (another site, for example).

## **To Use MCP Features**

1. **Enable Orchestration Module**:  
    Go to your **Server Modules** and activate the **Orchestration** module.
2. After that, in your **Settings** tab, the **orchestration** section will appear.
3. If you want your server to be used as an MCP (Master Control Point), go to the **Remote Access** section.

https://ai.thehiddendocs.com/mcp/