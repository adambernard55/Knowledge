# Here’s How I Built an MCP to Automate My Data Science Job

A step-by-step guide to building a Model Context Protocol (MCP) for your data science team (and why adapting AI is the key to staying ahead in this field).

Most of my days as a data scientist look like this:

- **Stakeholder**: "Can you tell us how much we made in advertising revenue in the last month and what percentage of that came from search ads?"
- **Me**: "Run an SQL query to extract the data and hand it to them."
- **Stakeholder**: "I see. What is our revenue forecast for the next 3 years?"
- **Me**: "Consolidate data from multiple sources, speak to the finance team, and build a model that forecasts revenue."

Tasks like the above are ad hoc requests from business stakeholders. They take around 3–5 hours to complete and are usually unrelated to the core project I'm working on.

When data-related questions like these come in, they often require me to push the deadlines of current projects or work extra hours to get the job done. And this is where AI comes in.

Once AI models like **[ChatGPT](https://openai.com/chatgpt)** and **[Claude](https://claude.ai/)** were made available, the team's efficiency improved, as did my ability to respond to ad hoc stakeholder requests. AI dramatically reduced the time I spent writing code, generating SQL queries, and even collaborating with different teams for required information. Additionally, after AI code assistants like **[Cursor](https://www.cursor.com/)** were integrated with our codebases, efficiency gains improved even further. Tasks like the one I just explained above could now be completed twice as fast as before.

Recently, when MCP servers started gaining popularity, I thought to myself:

> Can I build an MCP that automates these data science workflows further?

I spent two days building this MCP server, and in this article, I will break down:

- The results and how much time I've saved with my data science MCP
- Resources and reference materials used to create the MCP
- The basic setup, APIs, and services I integrated into my workflow

## # Building a Data Science MCP

   
If you don't already know what an MCP is, it stands for Model Context Protocol and is a framework that allows you to connect a large language model to external services.  
[This video](https://youtu.be/bC3mIQWHZMQ?si=GQfqAfVDJiBocsPZ) is a great introduction to MCPs.

#### // The Core Problem

The problem I wanted to solve with my new data science MCP was:

> How do I consolidate information that is scattered across various sources and generate results that can directly be used by stakeholders and team members?

To accomplish this, I built an MCP with three components, as shown in the flowchart below:

   

![Data Science MCP Flowchart](https://www.kdnuggets.com/wp-content/uploads/Data-Science-MCP-Flowchart.png)  
Image by Author | Mermaid

#### // Component 1: Query Bank Integration

As a knowledge base for my MCP, I used my team's query bank (which contained questions, a sample query to answer the question, and some context about the tables).

When a stakeholder asks me a question like this:

**What percentage of advertising revenue came from search ads?**

I no longer have to look through multiple tables and column names to generate a query. The MCP instead searches the query bank for a similar question. It then gains context about the relevant tables it should query and adapts these queries to my specific question. All I need to do is call the MCP server, paste in my stakeholder's request, and I get a relevant query in a few minutes.

#### // Component 2: Google Drive Integration

Product documentation is usually stored in Google Drive—whether it's a slide deck, document, or spreadsheet.

I connected my MCP server to the team's Google Drive so it had access to all our documentation across dozens of projects. This helps quickly extract data and answer questions like:

**Can you tell us how much we made in advertising revenue in the last month?**

I also indexed these documents to extract specific keywords and titles, so the MCP simply has to go through the keyword list based on the query rather than accessing hundreds of pages at once.

For example, if someone asks a question related to "mobile video ads," the MCP will first search through the document index to identify the most relevant files before looking through them.

#### // Component 3: Local Document Access

This is the simplest component of the MCP, where I have a local folder that the MCP searches through. I add or remove files as needed, allowing me to add my own context, information, and instructions on top of my team's projects.

## # Summary: How My Data Science MCP Works

   
Here's an example of how my MCP currently works to answer ad hoc data requests:

- A question comes in: "How many video ad impressions did we serve in Q3, and how much ad demand do we have relative to supply?"
- The document retrieval MCP searches our project folder for "Q3," "video," "ad," "demand," and "supply," and finds relevant project documents
- It then retrieves specific details about the Q3 video ad campaign, its supply, and demand from team documents
- It searches the query bank for similar questions about ad serves
- It uses the context obtained from the documents and query bank to generate an SQL query about Q3's video campaign
- Finally, the query is passed to a separate MCP that is connected to Presto SQL, which is automatically executed
- I then gather the results, review them, and send them to my stakeholders

## # Implementation Details

   
Here is how I implemented this MCP:

#### // Step 1: Cursor Installation

I used Cursor as my MCP client. You can install Cursor from [this link](https://cursor.com/). It is essentially an AI code editor that can access your codebase and use it to generate or modify code.

### // Step 2: Google Drive Credentials

Almost all the documents used by this MCP (including the query bank) were stored in Google Drive.

To give your MCP access to Google Drive, Sheets, and Docs, you'll need to set up API access:

1. Go to the Google Cloud Console and create a new project.
2. Enable the following APIs: Google Drive, Google Sheets, Google Docs.
3. Create credentials (OAuth 2.0 client ID) and save them in a file called `credentials.json`.

### // Step 3: Set Up FastMCP

**[FastMCP](https://github.com/jxnl/fastmcp)** is an open-source Python framework used to build MCP servers. I followed [this tutorial](https://youtu.be/rnljvmHorQw?si=_OmIlhq567Mwmng5) to build my first MCP server using FastMCP.

(Note: This tutorial uses Claude Desktop as the MCP client, but the steps are applicable to Cursor or any AI code editor of your choice.)

With FastMCP, you can create the MCP server with Google integration (sample code snippet below):

```
@mcp.tool()
def search_team_docs(query: str) -> str:
    """Search team documents in Google Drive"""
    drive_service, _ = get_google_services()
    # Your search logic here
    return f"Searching for: {query}"
```

### // Step 4: Configure the MCP

Once your MCP is built, you can configure it in Cursor. This can be done by navigating to Cursor's Settings window → Features → Model Context Protocol. Here, you'll see a section where you can add an MCP server. When you click on it, a file called `mcp.json` will open, where you can include the configuration for your new MCP server.

This is an example of what your configuration should look like:

```
{
  "mcpServers": {
    "team-data-assistant": {
      "command": "python",
      "args": ["path/to/team_data_server.py"],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "path/to/credentials.json"
      }
    }
  }
}
```

After saving your changes to the JSON file, you can enable this MCP and start using it within Cursor.

## # Final Thoughts

   
This MCP server was a simple side project I decided to build to save time on my personal data science workflows. It isn't groundbreaking, but this tool solves my immediate pain point: spending hours answering ad hoc data requests that take away from the core projects I'm working on. I believe that a tool like this simply scratches the surface of what's possible with generative AI and represents a broader shift in how data science work gets done.

The traditional data science workflow is moving away from:

- Spending hours finding data
- Writing code
- Building models

The focus is shifting away from hands-on technical work, and data scientists are now expected to look at the bigger picture and solve business problems. In some cases, we are expected to oversee product decisions and step in as a product or project manager.

As AI continues to evolve, I believe that the lines between technical roles will become blurred. What will remain relevant is the skill of understanding business context, asking the right questions, interpreting results, and communicating insights. If you are a data scientist (or an aspiring one), there is no question that AI will change the way you work.

You have two choices: you can either adopt AI tools and build solutions that shape this change for your team, or let others build them for you.