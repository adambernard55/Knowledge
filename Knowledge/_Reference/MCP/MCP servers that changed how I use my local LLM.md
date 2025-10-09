
3 MCP servers that changed how I use my local LLM and made it better than the cloud
A MacBook air connected to a monitor running DeepSeek-R1 locally
Follow

Adam Conway
1 day ago

Model Context Protocol has quietly become one of the most important pieces of the local AI puzzle, and there are some local servers I've found that I simply can't use my local AI without, now. For the uninitiated, think of MCP as a translator between your local large language model (LLM) and the rest of the digital world. Instead of manually pasting snippets from your browser, music player, or notes app, an MCP server can surface that data to your model automatically.

After months of tinkering, I've found three MCP servers that I simply can't use my local LLM without anymore. These three all work with OpenWebUI or LM Studio, and they're perfect for filling in the gaps in what cloud-based LLMs can do and them some, all while respecting your privacy and running locally.

How you use these will differ depending on your setup, but to use all of these in LM Studio, you'll need to install uv and npm.

Be careful when using MCP servers. MCP servers essentially abstract data from an API to a format that a model understands, and as such, many of them have access to the private data contained in your application. Only use MCP servers that you trust.

Search the web
Using SearXNG MCP to search for the latest SPRINTS album and receiving up to date information
This is by far one of the best MCP servers you can deploy, as it bridges the gap beautifully between your local language model and what a cloud-based provider can do. Using your own self-hosted SearXNG instance, you can provide search capabilities to any language model capable of tool usage. It gives my LLM the ability to look up fresh information from any web resource, without requiring it to be a part of its knowledge base.

SearXNG-MCP effectively turns your model into a local version of Perplexity or ChatGPT-with-browsing. I can ask about recent news, GitHub projects, or obscure Python libraries, and it'll return structured, sourced snippets. Everything stays in my network, and I retain control over what gets indexed or cached thanks to the fact that SearXNG is also a privacy-respecting search engine.

As long as SearXNG has access to the internet, the MCP server allows for remote access to the web. You can firewall your LLM and only allow it access to SearXNG if you want, and it'll still work. Just make sure to enable the "json" format in SearXNG's settings.yml file.

This is the configuration I use in LM Studio:

"searxng": {
"command": "npx",
"args": [
"-y",
"mcp-searxng"
],
"env": {
"SEARXNG_URL": "http://xxx:xxx"
}
}
Spotify-MCP
Control your Spotify and get music details
Spotify-MCP is pure quality-of-life magic. It lets my LLM see what I'm currently listening to on Spotify, fetch playlists, and make recommendations based on that context. You can even pair it with SearXNG, and so long as you explicitly say "use the Spotify tool to check X, then search the web for Y", your model should be able to join those tools together in order to research new music for you.

The one thing to be aware of when chaining commands like this is the context length. As you can see above, I asked about song recommendations, and it recommended Rabbit Run by IDLES. However, it failed the first time as Spotify wasn't open, though it thought there was a restriction. Once I opened Spotify, it still failed, likely because I had overflowed the context window. This causes the model to then start using approximations and can lead to hallucinations, and I had to prompt it again and say that it is available.

This runs entirely through the Spotify API. It requires you to create a Spotify app in the developer console, though it's completely free to do so. Once you've done that, it can access everything on Spotify and control your account, and it can even create playlists for you.

This is the configuration I use in LM Studio:

"spotify": {
"command": "uvx",
"args": [
"--python",
"3.12",
"--from",
"git+https://github.com/varunneal/spotify-mcp",
"spotify-mcp"
],
"env": {
"SPOTIFY_CLIENT_ID": "xxx",
"SPOTIFY_CLIENT_SECRET": "xxx",
"SPOTIFY_REDIRECT_URI": "xxx"
}
}
MCP-Obsidian
Link to your Obsidian vault, save notes, or search
Using LM Studio and an MCP server to save music recommendations to a note in Obsidian
Of all the MCP servers I've tested, Obsidian-MCP is arguably the most powerful and the one that truly transforms how I use my LLM. It links my model directly with my Obsidian vault, allowing me to query my knowledge base and pull information as I need it. I can search by tag, or update notes with more information.

Again, though, where this truly shines is when I pair it with another tool. Using Spotify-MCP, I can ask for song recommendations, and then have them saved to my Obsidian vault in Markdown format with a link to the track. It's actually really good, and I've already found two new artists that I'd never heard of before that I really like as a result. One of the songs recommended had 2,300 plays, and the other had 4,200. Not bad!

The key advantage is that everything stays offline. The model reads structured metadata and note content from Obsidian's local Markdown files, not a cloud API. Notes can be inherently personal and sensitive, so it's nice to be able to use this without worrying about my details going to the cloud. It uses Obsidian's REST API plugin to control your vault, so it's really easy to get set up and running.

This is the configuration I use in LM Studio:

"mcp-obsidian": {
"command": "uvx",
"args": [
"mcp-obsidian"
],
"env": {
"OBSIDIAN_API_KEY": "x",
"OBSIDIAN_HOST": "localhost",
"OBSIDIAN_PORT": "27123"
}
}
MCP servers can be standalone or chained together
These three servers form the entire basis of my local AI environment. Each one connects my LLM to a different slice of my personal world: the open web, my music, and my notes. Together, they turn what would otherwise be a static model into a dynamic, deeply personalized system. What's more, it's honestly a better experience than anything I could ever do with a cloud-based model, as I'd never give this kind of access to a cloud-based model.

The beauty of MCP is that it's entirely modular. You can run these servers individually or chain them together for multi-source reasoning, just like I did here. It's the missing piece for anyone serious about running local AI without sacrificing utility or privacy, and at the bare minimum, SearXNG is highly worth setting up and using with your local LLM just for web searches alone.

If you've built a local LLM setup and it still feels disconnected, start with these three MCP servers. Even with the relatively small model I'm using here (compared to the likes of GPT-5, anyway), it's incredible how it still manages to be a better personal experience than I've ever had with a cloud-based model.

Software and Services
Software and Services
AI
Follow

Like
Share
Thread
5
We want to hear from you! Share your opinions in the thread below and remember to keep it respectful.

Reply / Post
Sort by:

Popular
User Display Picture
Gnoyance
Which local model is your go to? Not many actually support MCP tool calls very well.

2025-10-08 15:00:34



Copy
User Display Picture
Danielo
You should provide a link to what obsidian MCP server you use. There are several

2025-10-08 11:27:18



1
Copy
User Display Picture
XDA logo
Adam
If you look at the LM Studio snippets for each, those will tell you which I'm using! There is only one Python package called mcp-obsidian, and there's nothing to install. LM Studio pulls it in with uvx.

2025-10-08 12:05:31



Copy
User Display Picture
Dan
Might help describing your setup, I assume using voice assistant preview with home assistant and ollama plugin. I mean you could toss mcp server in front of ollama, a bit confused how you are running 3 mcp at same time controlling with your voice.



Did you just code a python mcp server to redirect to each of those 3 based on context? Maybe running lm studio on your Ubuntu instance headless and pointing home assistant ollama to that port.



Trying to think how I'd pull it off lol. I'd probably code a simple python fastmcp to link them together.

2025-10-08 05:03:25



1
Copy
User Display Picture
XDA logo
Adam
This is all separate to my Home Assistant setup! This is just running in LM Studio on my PC. LM Studio allows you to define tools (which I gave the snippets before so that you can do it too) and the model will dynamically choose which to use. For example, you can see my requests where I say things like "use the searxng tool..." in the above screenshots.

2025-10-08 06:31:21

