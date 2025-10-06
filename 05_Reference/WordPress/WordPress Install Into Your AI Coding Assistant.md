The core concept is to enable the **Model Context Protocol (MCP)** on a local WordPress site using two primary tools: the **MCP Adapter** plugin and the experimental **Abilities API**.

### Key Concepts and Objectives

- **Model Context Protocol (MCP):** Developed by Anthropic, MCP is described as a "USB connector" or "USB hub" for AI. It allows external software (like a WordPress site's database or files) to connect to a Large Language Model (LLM) to provide context. This context goes beyond just the code files a developer is working on.
- **The Problem Solved:** Normally, when using an AI coding assistant (like GitHub Copilot in VS Code), the AI only has context of the code files. MCP enables the AI to access site-specific data, such as products in a WooCommerce database, or information about the client's site setup.
- **Learning Objectives:** The workshop aimed to:
    1. Introduce MCP.
    2. Show how to configure a WordPress site as an **MCP server** using the MCP Adapter plugin.
    3. Discuss the **Abilities API**.
    4. Show how to enable "abilities" as **MCP tools**.

### Essential Components and Setup

The process requires several components and specific steps:

#### 1. Software Requirements

Attendees needed:

- A laptop.
- **Node.js** (to use `npx`).
- A local WordPress installation (the presenter used WordPress Studio).
- An AI-powered code editor with MCP support, such as **VS Code with GitHub Copilot** (available on the free tier).

#### 2. WordPress Plugins and APIs

- **Abilities API:** This plugin (which is planned to be shipped in **WordPress 6.9**) enables the MCP tooling. It creates a standardized way to request abilities from a WordPress site and expose them via a REST API.
- **MCP Adapter:** This plugin runs on the WordPress site and essentially enables the site as an MCP server. It requires dependencies installed via **Composer**.

#### 3. Configuring the Server (The Process)

1. **Install Plugins:** Download and install the Abilities API and MCP Adapter plugins.
2. **Run Composer:** Navigate to the MCP Adapter folder and run `composer install` to handle dependencies.
3. **Set Permalinks:** To prevent the REST API endpoint for the MCP server from failing, the permalink setting must be set to anything other than "Plain".
4. **Register the Server:** In a custom plugin (e.g., `AI Experiments`), register the MCP server by hooking into the `mcp_adapter_init` action. This sets the REST API namespace and route (e.g., `ai_experiments/mcp`).
5. **Create an Ability (Tool):** Use the `wp_register_ability` function to create functionality (an "ability") that the AI can use.
    - **Abilities** are defined with a label, a description, an execute callback function, and crucial **input/output schemas**.
    - The schema is vital because it acts as the documentation, allowing the AI to understand what data the ability expects (`input schema`) and what data it returns (`output schema`).
    - A simple example ability shown was `get_site_info`, which returns a JSON object containing the site name, URL, active plugins, and WordPress version by using internal WordPress functions.
6. **Expose the Ability:** The ability must be explicitly added to the MCP server's list of exposed **tools** (or **resources** or **prompts**) within the registration code so the adapter knows to adapt it.
7. **Configure VS Code:** Create an `.vscode/mcp.json` file in the project root. This configuration points the AI code editor (like Copilot) to the WordPress site's MCP endpoint.
8. **Authentication:** The configuration uses a **WordPress Application Password** (which can be created per user) and the user's username for authentication, although the presenter noted this is not recommended for production (where OAuth would be preferred).
9. **Start the Server:** Use the `npx` command (managed automatically by VS Code's configuration) to start the server and handle the communication transport.

### Use Cases and Potential

Once configured, the AI coding assistant can use the custom abilities/tools created on the local WordPress site.

- **Basic Use Cases:** Getting site information (version, active plugins, themes).
- **Advanced Use Cases:**
    - Fetching the debug log when an error occurs, analyzing it, and suggesting fixes.
    - Running **security checks** on plugins while coding and returning the results to the AI for automated fixes.
    - Automating content workflows, such as generating a newsletter that pulls in recent posts and formats them correctly.
    - Creating or updating posts.
    - Performing site management tasks, like updating plugins across multiple sites.

The presentation concluded by encouraging developers to start building abilities now, as the Abilities API is coming to WordPress core soon, opening up a "blank slate" for "chat enabled automations".