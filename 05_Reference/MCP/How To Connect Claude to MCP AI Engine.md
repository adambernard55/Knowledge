## How To Connect Claude to WordPress with MCP

 [May 24, 2025](https://meowapps.com/claude-wordpress-mcp/)

**What’s MCP?** The [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) is the open standard Claude (and soon other AI assistants) uses to call external “tools.” An MCP server tells Claude which tools it offers, and Claude can then call them over JSON-RPC.

With **AI Engine**, your WordPress site publishes **30+ tools** . Everything from `wp_create_post` to `wp_upload_media` and theme operations. Claude can draft posts, upload images, tag content, even fork and modify a theme, all from one chat! 🙀

Want examples? Sure! Both [pacman.meowapps.com](https://pacman.meowapps.com/) and [mcp.meowapps.com](https://mcp.meowapps.com/) are live WordPress sites built entirely using Claude and AI Engine, with no extra plugins or themes. They’re personal test setups — one a retro gaming site, the other a scratchpad I update as I experiment with new features. They show exactly what’s possible with just a clean WordPress install and the MCP bridge.

The little Node script `mcp.js` (bundled with AI Engine) is the bridge. **It was written on MacOS so it has not been tested on Windows.** What it does:

- Opens a secure **SSE stream** to WordPress (`/wp-json/mcp/v1/sse`);
- Tunnels Claude’s JSON-RPC calls to `/wp-json/mcp/v1/messages`;
- Relays the responses back to Claude;
- Cleans up automatically (sends `mwai/kill` and exits) when Claude quits.

At first, I actually built everything using `mcp-remote`, but I found it pretty hard to debug, especially when dealing with _remote_ MCP servers. So I ended up making my own version in `mcp.js`. But if you prefer, you can totally use `mcp-remote` — it does work just fine with the MCP implementation in AI Engine too!

No doubt that Claude will eventually support direct connections to AI Engine with SSE. OpenAI already said they plan to bring that to ChatGPT too. But for now, the whole stack is still in beta. It’s really meant for advanced users and developers who are comfortable with the command line and debugging. Plus, it gives the AI full access to your WordPress, so… anything can happen!

## **1. Requirements**

1. **WordPress 6.7+**
2. **WP REST API** (normally enabled by default)
3. **AI Engine** **2.7.6+**
4. **Claude Desktop ≥ 0.9.2**
5. **Node ≥ 20.19.0**

`mcp.js` also handles everything else: it registers your sites, edits Claude’s config, launches the relay, and shuts it down cleanly.

## **2. Connect Claude to your site**

First, set a token for the authentication. You’ll find this field in Settings of AI Engine, under the **DevTools** tab.

You’ll want to grab the content of the labs folder onto your local machine (or maybe just the `mcp.js` file, actually). Run these two commands:

```bash
# Let's make it an executable
chmod +x ai-engine/labs/mcp.js
# Register site + patch Claude config
ai-engine/labs/mcp.js add https://example.com TOKEN
# First-time test (verbose)
ai-engine/labs/mcp.js start example.com
```

The first command writes an MCP entry to Claude’s config: `~/Library/Application Support/Claude/claude_desktop_config.json`.

Launch Claude Desktop and wait a few seconds. You should be able to see that AI Engine is properly loaded as a MCP server:

![](https://meowapps.com/wp-content/uploads/Claude-with-AI-Engine-1024x636.png)

But honestly, don’t be surprised if it doesn’t… you have no idea how many issues I ran into. And most of the time, it wasn’t even about the code, but stuff like server security, SSL, caching, SSE quirks, edge-caching, Cloudflare headaches, and so on. Also, if the token doesn’t work, Claude will not tell you anything, you’ll have to look into its logs! I really recommend starting with a local install you can fully control, just to have a clean, simple environment at first.

## **3. Try these prompts**

- _Simple —_ “List my latest 5 posts.”
- _Simple —_ “Create a draft post titled **My AI Journey**, one paragraph, and attach a random Media-Library image.”
- _Intermediate —_ “Review the 10 newest posts, then publish a logical follow-up. Re-use existing categories & tags. Generate an image if none fits.”
- _Advanced —_ “Fork **Twenty Twenty-One** into a dark grid theme called **Futurism** supporting post types Article & Project.”

## **4. Issues**

Take a look inside the `~/Library/Logs/Claude` directory. There are some log files there that might be useful — hopefully! Though honestly, they’re not as helpful as I wish they were.

Each SSE connection ties up one PHP worker. Kinsta and similar hosts give 5–8 workers per site, so two Claude sessions can chew through resources fast if they don’t end properly. You might see that your website doesn’t load anymore, in that case you’ll have to restart the web server, or kill the php-fm processes, that highly depends on how your server is set up.

If the SSE connection remains open unintentionally, use the provided graceful exit (closing Claude or terminating the relay with Ctrl+C). Otherwise, manually restart your PHP or web server processes.

If you’re running into JS errors and using NVM, double-check that there aren’t any old Node versions still listed when you run `nvm list`. For some reason, NVM can end up loading an older version instead of your default or latest one, and that can cause all sorts of issues.

Caching can cause all kinds of problems. Since you’re not connecting to the SSE through your browser, Cloudflare or your hosting service might flag you as a non-browser and then SSE just won’t work (yeah, that really happens). Usually it’s done for caching reasons. For example, if you’re using Kinsta Edge-Caching, you’ll need to turn it off completely. Or you can talk to Kinsta and ask them to disable it only for the SSE endpoint of AI Engine.

If you want to test your server for SSE, try my gist [here](https://gist.github.com/jordymeow/4854992e7b4c510083c08ef8f5ca82c3).

### **Handy `mcp.js` commands**

```php
# Disable auto-launch
ai-engine/labs/mcp.js claude none
# human-readable relay
mcp.js start example.com
# silent relay (Claude uses this)
mcp.js relay example.com
# manage sites
mcp.js add mysite.com TOKEN
# show all
mcp.js list
# switch target
mcp.js claude mysite.com
# ad-hoc RPC (no curl)
mcp.js post mysite.com '{\"method\":\"tools/list\"}' <session_id>
```

### **Extra PHP logging (optional)**

Edit `wp-content/plugins/ai-engine/classes/modules/mcp.php`:

```php
private $logging = true;
```

Then tail `wp-content/debug.log`.

### **What happens when you close Claude?**

Claude doesn’t send [SIGTERM](https://meowapps.com/%22https://nodejs.org/api/process.html#signal_events\%22) to helpers, so `mcp.js` handles its own cleanup:

- detects stdin close,
- sends a `mwai/kill` notification to WordPress,
- aborts the SSE fetch,
- exits 0 — no orphan `node` processes.

This entry was posted in [Learn](https://meowapps.com/category/learn/). Bookmark the [permalink](https://meowapps.com/claude-wordpress-mcp/).

https://meowapps.com/claude-wordpress-mcp/