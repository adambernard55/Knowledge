
Published on Oct 1


GEM, an open-source environment simulator, facilitates experience-based learning for large language models by providing a standardized framework and diverse environments for training and benchmarking reinforcement learning algorithms.

The training paradigm for large language models (LLMs) is moving from static datasets to experience-based learning, where agents acquire skills via interacting with complex environments. To facilitate this transition we introduce GEM (General Experience Maker), an open-source environment simulator designed for the age of LLMs. Analogous to OpenAI-Gym for traditional reinforcement learning (RL), GEM provides a standardized framework for the environment-agent interface, including asynchronous vectorized execution for high throughput, and flexible wrappers for easy extensibility. GEM also features a diverse suite of environments, robust integrated tools, and single-file example scripts demonstrating using GEM with five popular RL training frameworks. Along with this, we also provide a set of baselines across 24 environments using REINFORCE with Return Batch Normalization (ReBN), which -- unlike GRPO -- is compatible with the full RL setting of dense per-turn rewards and offers better credit assignment. We further conduct apple-to-apple benchmarking of PPO, GRPO and REINFORCE in both single- and multi-turn settings using GEM to shed light on the algorithmic designs. Lastly, GEM also functions as a convenient evaluation toolkit besides a training environment. We hope this framework can help accelerate future agentic LLM research.

Community

taesiri
Paper submitter
2 days ago

The training paradigm for large language models (LLMs) is moving from static datasets to experience-based learning, where agents acquire skills via interacting with complex environments. To facilitate this transition we introduce GEM (General Experience Maker), an open-source environment simulator designed for the age of LLMs. Analogous to OpenAI-Gym for traditional reinforcement learning (RL), GEM provides a standardized framework for the environment-agent interface, including asynchronous vectorized execution for high throughput, and flexible wrappers for easy extensibility. GEM also features a diverse suite of environments, robust integrated tools, and single-file example scripts demonstrating using GEM with five popular RL training frameworks. Along with this, we also provide a set of baselines across 24 environments using REINFORCE with Return Batch Normalization (ReBN), which -- unlike GRPO -- is compatible with the full RL setting of dense per-turn rewards and offers better credit assignment. We further conduct apple-to-apple benchmarking of PPO, GRPO and REINFORCE in both single- and multi-turn settings using GEM to shed light on the algorithmic designs. Lastly, GEM also functions as a convenient evaluation toolkit besides a training environment. We hope this framework can help accelerate future agentic LLM research.

Reply

librarian-bot
about 21 hours ago

This is an automated message from the Librarian Bot. I found the following papers similar to this paper.

The following papers were recommended by the Semantic Scholar API

VerlTool: Towards Holistic Agentic Reinforcement Learning with Tool Use (2025)
Agentic Reinforcement Learning with Implicit Step Rewards (2025)
Enhancing Vision-Language Model Training with Reinforcement Learning in Synthetic Worlds for Real-World Success (2025)
AgentGym-RL: Training LLM Agents for Long-Horizon Decision Making through Multi-Turn Reinforcement Learning (2025)
Process-Supervised Reinforcement Learning for Interactive Multimodal Tool-Use Agents (2025)
Efficient Multi-turn RL for GUI Agents via Decoupled Training and Adaptive Data Curation (2025)
SFR-DeepResearch: Towards Effective Reinforcement Learning for Autonomously Reasoning Single Agents (2025)
Please give a thumbs up to this comment if you found it helpful!

If you want recommendations for any Paper on Hugging Face checkout this Space

You can directly ask Librarian Bot for paper recommendations by tagging it in a comment: @librarian-bot recommend

Reply
Tap or paste here to upload images
Sign up or log in to comment

Models citing this paper
0
No model linking this paper

Cite arxiv.org/abs/2510.01051 in a model README.md to link it from this page.
Datasets citing this paper
0
No dataset linking this paper

Cite arxiv.org/abs/2510.01051 in a dataset README.md to link it from this page.
Spaces citing this paper
0
No Space linking this paper

Cite arxiv.org/abs/2510.01051 in a Space README.md to link it from this page.
Collections including this paper
