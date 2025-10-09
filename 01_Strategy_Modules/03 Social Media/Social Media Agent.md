# Social Media Agent → Using Nebius, Composio and Memori

### Sep'25 - Issue #66

[](https://substack.com/@astrodevil)

[Mr. Ånand](https://substack.com/@astrodevil)

Sep 22, 2025

[](https://mranand.substack.com/p/social-media-agent-using-nebius-composio/comments)

```
Welcome to the AI Insights tribe. Join others who are receiving high-signal stories from the AI world. In case you missed it, Subscribe to our newsletter.
```


---

AI agents are no longer just about answering questions — they’re starting to take on entire workflows. One of the most exciting use cases? Letting AI handle your social media while you focus on building.

In this guide, we’ll show you how to build a Twitter agent that connects to your account, learns your writing style, and posts on your behalf. You simply give it a topic, and it generates tweets that sound like _you_. It’s a perfect example of how powerful AI agents become when they’re paired with the right tools and memory.

---

[

![](https://substackcdn.com/image/fetch/$s_!1v9Y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7fcc8476-4fe7-4687-9611-94a02a2e8cc5_1920x1080.png)



](https://substackcdn.com/image/fetch/$s_!1v9Y!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7fcc8476-4fe7-4687-9611-94a02a2e8cc5_1920x1080.png)

Our Final App with Streamlit UI

### The Problem We're Solving

Let's be honest, maintaining an active social media presence is exhausting. You want to stay visible, share insights, and engage with your community, but you also have actual work to do.

Most AI writing tools give you generic, robotic content that screams "ChatGPT wrote this." But what if your AI could actually learn your voice, remember your style, and generate content that sounds authentically you?

**That's what we built, a social media agent that:**

1. Scrapes your most viral tweets to understand your style
    
2. Stores your personality profile in persistent memory
    
3. Generates new tweets that match your exact tone and voice
    
4. Posts directly to Twitter with one click
    

---

### The Tech Stack

Before we proceed, let's break down each tool we're using and why they're perfect for this project:

#### 1. [Nebius AI](https://studio.nebius.com/)

**What it is:** A platform that hosts multiple AI models (text-to-text, embedding, text-to-image, etc) at very affordable costs.

**Why we chose it:** It offers a wide variety of models to choose from at extremely low costs, making it perfect for experimentation and production use.

Get started: Go to [Nebius Studio](https://studio.nebius.com/) and copy your API key.

#### 2. [ScrapeGraph (SGAI)](https://scrapegraphai.com/)

**What it is:** An AI-powered web scraping service that extracts structured data from any website.

**Why we chose it:** Traditional web scraping is fragile and breaks when websites change. ScrapeGraph uses AI to understand page content and extract precisely what you need, in this case, your tweet history, regardless of layout changes on Twitter.

Get started: Visit [ScrapeGraph Dashboard](https://dashboard.scrapegraphai.com/), sign up, and get your API key.

#### 3. [Composio](https://composio.dev/)

**What it is:** A platform providing ready-to-use integrations for AI agents, including Twitter posting capabilities.

**Why we chose it:** Instead of wrestling with Twitter's API authentication and rate limits, Composio gives us a clean, reliable way to post tweets programmatically. It handles all the OAuth complexity for us.

Get started: Sign up at [Composio](https://composio.dev/), create a new project, set up Twitter authentication, then copy your Composio API key, Twitter auth config ID, and user ID.

#### 4. [Memori](https://memori.gibsonai.com/)

**What it is:** An intelligent memory layer framework that gives AI agents persistent, human-like memory.

**Why we chose it:** This is the secret sauce. Memori stores your Twitter style profile permanently, so your agent remembers exactly how you write, even across different sessions. No more re-analyzing your style every time.

**We already have this project on [GitHub](https://github.com/Arindam200/awesome-ai-apps/tree/main/memory_agents/social_media_agent), You can clone it or follow our walkthrough below!**

### Architecture Overview

[

![](https://substackcdn.com/image/fetch/$s_!gTGb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbe8fdcf5-ec26-4baf-b2cd-7ad5e76c57cd_2659x1005.png)



](https://substackcdn.com/image/fetch/$s_!gTGb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbe8fdcf5-ec26-4baf-b2cd-7ad5e76c57cd_2659x1005.png)

Our Social Media Agent

**Here's how the data flows:**

1. Input your Twitter handle or link
    
2. ScrapeGraph extracts your most popular tweets
    
3. Nebius AI analyzes your style across multiple dimensions
    
4. Memori stores your style profile permanently
    
5. When you want to post, just tell the agent your topic
    
6. It generates content in your style and posts via Composio
    

### Setting Up Your Agent App

#### Step 1: Environment Configuration

Create a `.env` file in your project directory and add your API keys:

```
# Nebius AI Configuration
NEBIUS_API_KEY=your_nebius_api_key

# ScrapeGraph Configuration
SGAI_API_KEY=your_scrapegraph_api_key

# Composio Twitter Integration
COMPOSIO_API_KEY=your_composio_api_key
TWITTER_AUTH_CONFIG_ID=your_twitter_auth_config_id
USER_ID=your_unique_user_identifier
```

#### Step 2: Install Dependencies

Create a `requirements.txt` file with these dependencies:

```
streamlit>=1.48.1
python-dotenv>=1.1.1
composio>=0.8.9
langchain-scrapegraph>=1.0.0
langchain-core>=0.3.0
memorisdk>=1.0.1
pypdf>=5.1.0
python-docx>=1.2.0
langchain-nebius>=0.1.3
```

Then install everything:

```
pip install -r requirements.txt
```

Or choose uv setup:

```
uv sync
```

#### Step 3: Get the Code

Now we'll create three main files: `twitter_agents.py`, `app.py`, and `create_tweet.py`.

Head over to our [GitHub repository](https://github.com/Arindam200/awesome-ai-apps/tree/main/memory_agents/social_media_agent) and copy the content of each file into your respective files.

**File breakdown:**

- `create_tweet.py`: Handles social media posting, connects Composio to your Twitter account and makes posts
    
- `twitter_agents.py`: Contains all other agents, ScrapeGraph for scraping tweets, Nebius for analysis, and Memori for storage
    
- `app.py`: The Streamlit application that brings everything together with a user-friendly interface
    

### Running Your Social Media Agent

Once you understand how each file works and how they're connected, run the application:

```
streamlit run app.py
```

Your browser will automatically open. If not, navigate to `http://localhost:8501`.

### Using the Agent

1. **Analyze Your Style**: In the sidebar, enter your Twitter username and click "Analyze Tweeting Style"
    
2. **Wait for Processing**: The system will scrape your 10 most viral tweets, analyze them through Nebius, and store your style profile in Memori
    
3. **Generate Tweets**: In the main chat interface, tell it what you want to write about (e.g., "Write about the future of AI in healthcare")
    
4. **Review and Post**: Watch it write a tweet in your style, then click "Post Tweet"
    
5. **Verify**: Head over to your Twitter account and see your AI-generated tweet live!
    

Subscribe

---

### Wrap-Up

This isn't just another AI writing tool. The combination of persistent memory (Memori), intelligent scraping (ScrapeGraph), powerful language models (Nebius), and seamless posting (Composio) creates an agent that truly understands and replicates your voice.

The memory component is particularly crucial because your agent gets smarter over time and doesn't forget your style between sessions.

If you found this useful, You're free to contribute to this project if you want to add more features like the ability to post on:

✅ LinkedIn

✅ Instagram

✅ other platforms

👉 Follow our Newsletter on X: [Daily AI Insights](https://x.com/DailyAI_Insight)

---

### Last Week in AI

- [TTD-DR](https://research.google/blog/deep-researcher-with-test-time-diffusion/?utm_source=twitter&utm_medium=social&utm_campaign=social_post&utm_content=gr-acct) - Google launched new framework to achieves state-of-the-art results in writing long-form research reports and completing complex reasoning tasks.
    
- [Grok 4 Fast](https://x.ai/news/grok-4-fast) - xAI launched a multimodal reasoning model with a 2M context window that sets a new standard for cost-efficient intelligence.
    
- [Granite-Docling](https://www.ibm.com/new/announcements/granite-docling-end-to-end-document-conversion) - IBM launched **Granite-Docling-258M**, an ultra-compact and cutting-edge open-source vision-language model (VLM) for converting documents to machine-readable formats
    
- [Qwen3-ASR-Toolkit](https://x.com/Alibaba_Qwen/status/1968230660973396024) - A free, open-source CLI to transcribe HOURS-long audio/video files at high speed. Unleash the full power of the Qwen3-ASR-Flash API.
    
- [Production-ready AI agents with LangGraph](https://x.com/LangChainAI/status/1969431355613462888) - A New Deep Agents with LangGraph Course.