---
aliases:
  - |-
    Large Language Models
    How to Build a Powerful Deep Research System
---


Large Language Models
How to Build a Powerful Deep Research System
Learn how to access vasts amounts of information with your own deep research system

Eivind Kjosbakken
Oct 4, 2025

Deep research
In this article, I discuss how you can build your own deep research system. Image by ChatGPT.
Deep research is a popular feature you can activate in apps such as ChatGPT and Google Gemini. It allows users to ask a query as usual, and the application spends a longer time properly researching the question and coming up with a better answer than normal LLM responses.

You can also apply this to your own collection of documents. For example, suppose you have thousands of documents of internal company information, you might want to create a deep research system that takes in user questions, scans all the available (internal) documents, and comes up with a good answer based on that information.

Deep research system
This infographic highlights the main contents of this article. I’ll discuss in which situations you need to build a deep research system, and in which situations simpler approaches like RAG or keyword search are more suitable. Continuing, I’ll discuss how to build a deep research system, including gathering data, creating tools, and putting it all together with an orchestrator LLM and subagents. Image by ChatGPT.

Why do I need a deep research system?

This is a fair question, because there are other alternatives that are viable in many situations:

Feed all data into an LLM
RAG
Keyword search
If you can get away with these simpler systems, you should almost always do that. The by far easiest approach is simply feeding all the data into an LLM. If your information is contained in fewer than 1 million tokens, this is definitely a good option.

Furthermore, if traditional RAG works well, or you can find relevant information with a keyword search, you should also choose those options. However, sometimes, neither of these solutions is strong enough to solve your problem. Maybe you need to deeply analyze many sources, and chunk retrieval from similarity (RAG) isn’t good enough. Or you can’t use keyword search because you’re not familiar enough with the dataset to know which keywords to use. In which case, you should consider using a deep research system.

How to build a deep research system
You can naturally utilize the deep research system from providers such as OpenAI, which provides a Deep Research API. This can be a good alternative if you want to keep things simple. However, in this article, I’ll discuss in more detail how a deep research system is built up, and why it’s useful. Anthropic wrote a very good article on their Multi Agent Research System (which is deep research), which I recommend reading to understand more details about the topic.

Gathering and indexing information
The first step for any information finding system is to gather all your information in one place. Maybe you have information in apps like:

Google Drive
Notion
Salesforce
You then either need to gather this information in one place (convert it all to PDFs, for example, and store them in the same folder), or you can connect with these apps, like ChatGPT has done in its application.

After gathering the information, we now need to index it to make it easily available. The two main indices you should create are:

Keyword search index. For example BM25
Vector similarity index: Chunk up your text, embed it, and store it in a vectorDB like Pinecone
This makes the information easily accessible from the tools I’ll describe in the next session.

Tools
The agents we’ll be using later on need tools to fetch relevant information. You should thus make a series of functions that make it easy for the LLM to fetch the relevant information. For example, if the user queries for a Sales report, the LLM might want to make a keyword search for that and analyse the retrieved documents. These tools can look like this:

@tool 
def keyword_search(query: str) -> str:
    """
    Search for keywords in the document.
    """
    results = keyword_search(query)

    # format responses to make it easy for the LLM to read
    formatted_results = "\n".join([f"{result['file_name']}: {result['content']}" for result in results])

    return formatted_results


@tool
def vector_search(query: str) -> str:
    """
    Embed the query and search for similar vectors in the document.
    """
    vector = embed(query)
    results = vector_search(vector)

    # format responses to make it easy for the LLM to read
    formatted_results = "\n".join([f"{result['file_name']}: {result['content']}" for result in results])

    return formatted_results
You can also allow the agent access to other functions, such as:

Internet search
Filename only search
And other potentially relevant functions

Putting it all together
A deep research system typically consists of an orchestrator agent and many subagents. The approach is usually as follows:

An orchestrator agent receives the user query and plans approaches to take
Many subagents are sent to fetch relevant information and feed the summarized information back to the orchestrator
The orchestrator determines if it has enough information to answer the user query. If no, we go back to the last bullet point; if yes, we can provide for the final bullet point
The orchestrator puts all the information together and provides the user with an answer

This figure highlights the deep research system I discussed. You input the user query, an orchestrator agent processes it, and sends subagents to fetch info from the document corpus. The orchestrator agent then determines if it has enough information to respond to the user query. If the answer is no, it fetches more information, and if it has enough information, it generates a response for the user. Image by the author.
Furthermore, you might also have a clarifying question, if the user’s question is vague, or just to narrow down the scope of the user’s query. You’ve probably experienced this if you used any deep research system from a frontier lab, where the deep research system always starts off by asking a clarifying question.

Usually, the orchestrator is a larger/better model, for example, Claude Opus, or GPT-5 with high reasoning effort. The subagents are typically smaller, such as GPT-4.1 and Claude Sonnet.

The main advantage of this approach (over traditional RAG, especially) is that you allow the system to scan and analyze more information, lowering the chance of missing information that is relevant to respond to the user query. The fact that you have to scan more documents also typically makes the system slower. Naturally, this is a trade-off between time and quality of responses.

Conclusion
In this article, I have discussed how to build a deep research system. I first covered the motivation for building such a system, and in which scenarios you should instead focus on building simpler systems, such as RAG or keyword search. Continuing, I discussed the foundation for what a deep research system is, which essentially takes in a user query, plans for how to answer it, sends sub-agents to fetch relevant information, aggregates that information, and responds to the user.
