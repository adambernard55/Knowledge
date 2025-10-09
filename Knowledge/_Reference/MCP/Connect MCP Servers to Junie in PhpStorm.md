[Hanna Yakush](https://blog.jetbrains.com/author/hanna-yakush-jetbrains-com)

September 5, 2025

The **Model Context Protocol (MCP)** is an open standard introduced by [Anthropic](https://docs.anthropic.com/en/docs/mcp). Think of it as a USB-C port for AI: a consistent way to plug the AI models your AI agent uses into specific tools and data sources.

You can connect [Junie](https://www.jetbrains.com/junie/), the AI coding agent by JetBrains, to a wide variety of [officially provided or community-built MCP servers](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#-third-party-servers), or build your own MCP server using one of the available SDKs – including the [brand-new MCP PHP SDK](https://thephp.foundation/blog/2025/09/05/php-mcp-sdk).

## About the MCP PHP SDK

MCP SDKs are lightweight frameworks that handle the protocol details so that developers can focus on the application functionality that AI agents will use.

The [MCP PHP SDK](https://github.com/modelcontextprotocol/php-sdk) is an official MCP SDK built as a joint effort by **the** [**PHP Foundation**](https://thephp.foundation/)**, Anthropic’s MCP team, and** [**Symfony**](https://symfony.com/). The goal of the project is to provide a **framework-agnostic**, production-ready reference implementation that the PHP ecosystem can rely on. For details about using MCP PHP SDK or contributing to it, see [**modelcontextprotocol/php-sdk**](https://github.com/modelcontextprotocol/php-sdk).

## Junie and the MCP

Whether you are building your own MCP server or using an existing one, here’s how to make it work with the [Junie AI coding agent](https://www.jetbrains.com/junie/) in PhpStorm:

1. **Make sure that the MCP server can be accessed and started.**  
    The specific method for starting an MCP server depends on the server’s implementation – please refer to the MCP server’s documentation for instructions.  
    
2. **Configure Junie in PhpStorm to connect.**  
    To add the MCP server to Junie, press _⇧Shift_ twice to open the search window and search for “MCP Settings”. On the _MCP Settings_ page, you can see the list of connected servers and add or edit JSON configurations for the MCP servers in the `~/.junie/mcp.json` file.  
      
    ![](https://blog.jetbrains.com/wp-content/uploads/2025/09/mcp_settings_junie.png)  
    To configure an MCP server at the project level, manually add an `mcp.json` file with your desired server configurations to the `.junie/mcp` folder in the project root.  
      
    To view the list of actions that Junie can perform through a configured MCP server, locate the server in the _MCP Settings_ list and expand the _Status_ drop-down.  
      
    ![](https://blog.jetbrains.com/wp-content/uploads/2025/09/view_available_mcp_ser_tools.png)  
    For example, with the [**Laravel Boost MCP server**](https://boost.laravel.com/installed), Junie gets useful tools for working with Laravel projects, such as listing Artisan commands, routes, or config files, reading logs, querying databases, or searching versioned documentation.  
      
    This is where the MCP shines: It bridges the gap between LLMs and dynamic, framework- and project-specific context and data.  
    
3. **Benefit from improved context when using Junie.**   
    When running a prompt, Junie analyzes what commands registered with the configured MCP servers are relevant and executes them through the respective MCP server.  
      
    ![](https://blog.jetbrains.com/wp-content/uploads/2025/09/laravel-boost-junie.png)

The Junie coding agent keeps evolving. Join the conversation in our [Junie Discord community](https://jb.gg/junie/web) and help shape what’s next for Junie.

## A new chapter for MCP servers in PHP

While you could use any SDK implementation to create MCP servers for PHP projects, the official [MCP PHP SDK](https://github.com/modelcontextprotocol/php-sdk) boosts the contribution of the PHP community into the AI ecosystem, both within the PHP realm and beyond.

The MCP PHP SDK maintainers encourage PHP developers to submit their MCP integrations for PHP frameworks and tools – Laravel, Symfony, WordPress, custom APIs, and more – to be listed in the [SDK’s GitHub repo](https://github.com/modelcontextprotocol/php-sdk).