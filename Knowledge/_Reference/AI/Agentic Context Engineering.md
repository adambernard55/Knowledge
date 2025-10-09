
Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models
Published on Oct 6
Submitted by
taesiri
on Oct 7
#3 Paper of the day

Authors:

Qizheng Zhang
,
Changran Hu
,
Shubhangi Upasani
,
Boyuan Ma
,
Fenglu Hong
,
Vamsidhar Kamanuru
,
Jay Rainton
,
Chen Wu
,
Mengmeng Ji
,
Hanchen Li
,
Urmish Thakker
,
James Zou
,
Kunle Olukotun
Abstract
ACE, a framework for adaptive context engineering, enhances LLM applications by preserving detailed knowledge through structured updates, outperforming baselines in agent and domain-specific tasks with reduced adaptation costs.

Large language model (LLM) applications such as agents and domain-specific reasoning increasingly rely on context adaptation -- modifying inputs with instructions, strategies, or evidence, rather than weight updates. Prior approaches improve usability but often suffer from brevity bias, which drops domain insights for concise summaries, and from context collapse, where iterative rewriting erodes details over time. Building on the adaptive memory introduced by Dynamic Cheatsheet, we introduce ACE (Agentic Context Engineering), a framework that treats contexts as evolving playbooks that accumulate, refine, and organize strategies through a modular process of generation, reflection, and curation. ACE prevents collapse with structured, incremental updates that preserve detailed knowledge and scale with long-context models. Across agent and domain-specific benchmarks, ACE optimizes contexts both offline (e.g., system prompts) and online (e.g., agent memory), consistently outperforming strong baselines: +10.6% on agents and +8.6% on finance, while significantly reducing adaptation latency and rollout cost. Notably, ACE could adapt effectively without labeled supervision and instead by leveraging natural execution feedback. On the AppWorld leaderboard, ACE matches the top-ranked production-level agent on the overall average and surpasses it on the harder test-challenge split, despite using a smaller open-source model. These results show that comprehensive, evolving contexts enable scalable, efficient, and self-improving LLM systems with low overhead.

Community

taesiri
Paper submitter
2 days ago

Large language model (LLM) applications such as agents and domain-specific reasoning increasingly rely on context adaptation -- modifying inputs with instructions, strategies, or evidence, rather than weight updates. Prior approaches improve usability but often suffer from brevity bias, which drops domain insights for concise summaries, and from context collapse, where iterative rewriting erodes details over time. Building on the adaptive memory introduced by Dynamic Cheatsheet, we introduce ACE (Agentic Context Engineering), a framework that treats contexts as evolving playbooks that accumulate, refine, and organize strategies through a modular process of generation, reflection, and curation. ACE prevents collapse with structured, incremental updates that preserve detailed knowledge and scale with long-context models. Across agent and domain-specific benchmarks, ACE optimizes contexts both offline (e.g., system prompts) and online (e.g., agent memory), consistently outperforming strong baselines: +10.6% on agents and +8.6% on finance, while significantly reducing adaptation latency and rollout cost. Notably, ACE could adapt effectively without labeled supervision and instead by leveraging natural execution feedback. On the AppWorld leaderboard, ACE matches the top-ranked production-level agent on the overall average and surpasses it on the harder test-challenge split, despite using a smaller open-source model. These results show that comprehensive, evolving contexts enable scalable, efficient, and self-improving LLM systems with low overhead.

Reply

librarian-bot
1 day ago

This is an automated message from the Librarian Bot. I found the following papers similar to this paper.

The following papers were recommended by the Semantic Scholar API

Just-in-time Episodic Feedback Hinter: Leveraging Offline Knowledge to Improve LLM Agents Adaptation (2025)
Tool-R1: Sample-Efficient Reinforcement Learning for Agentic Tool Use (2025)
JoyAgent-JDGenie: Technical Report on the GAIA (2025)
How Can Input Reformulation Improve Tool Usage Accuracy in a Complex Dynamic Environment? A Study on τ-bench (2025)
ReasoningBank: Scaling Agent Self-Evolving with Reasoning Memory (2025)
SEDM: Scalable Self-Evolving Distributed Memory for Agents (2025)
ALAS: Autonomous Learning Agent for Self-Updating Language Models (2025)
Please give a thumbs up to this comment if you found it helpful!

If you want recommendations for any Paper on Hugging Face checkout this Space

You can directly ask Librarian Bot for paper recommendations by tagging it in a comment: @librarian-bot recommend

Reply
Tap or paste here to upload images
Sign up or log in to comment

Models citing this paper
0
No model linking this paper

Cite arxiv.org/abs/2510.04618 in a model README.md to link it from this page.
Datasets citing this paper
0
No dataset linking this paper

Cite arxiv.org/abs/2510.04618 in a dataset README.md to link it from this page.
Spaces citing this paper
0
No Space linking this paper

Cite arxiv.org/abs/2510.04618 in a Space README.md to link it from this page.
Collections including this paper
2
AI-paper
Collection
293 items
•
Updated about 16 hours ago
•
1
Learning from examples - training/inference
Collection
4 items
•
Updated about 8 hours ago
•
1
System theme
Company
TOS
Privacy
About
Jobs
Website
Models
Datasets
Spaces
Pricing
Docs