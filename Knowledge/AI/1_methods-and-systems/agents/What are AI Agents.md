
We keep talking about AI agents, but do we ever know what they are?

Sean Falconer, Confluent
October 12, 2025
CleoJ/Agent autonomy




Imagine you do two things on a Monday morning.

First, you ask a chatbot to summarize your new emails. Next, you ask an AI tool to figure out why your top competitor grew so fast last quarter. The AI silently gets to work. It scours financial reports, news articles and social media sentiment. It cross-references that data with your internal sales numbers, drafts a strategy outlining three potential reasons for the competitor's success and schedules a 30-minute meeting with your team to present its findings.

AI IMPACT: Architect for Agentic Efficiency to Drive AI Profitability


We're calling both of these "AI agents," but they represent worlds of difference in intelligence, capability and the level of trust we place in them. This ambiguity creates a fog that makes it difficult to build, evaluate, and safely govern these powerful new tools. If we can't agree on what we're building, how can we know when we've succeeded?

This post won't try to sell you on yet another definitive framework. Instead, think of it as a survey of the current landscape of agent autonomy, a map to help us all navigate the terrain together.


What are we even talking about? Defining an "AI agent"
Before we can measure an agent's autonomy, we need to agree on what an "agent" actually is. The most widely accepted starting point comes from the foundational textbook on AI, Stuart Russell and Peter Norvig’s “Artificial Intelligence: A Modern Approach.” 

They define an agent as anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators. A thermostat is a simple agent: Its sensor perceives the room temperature, and its actuator acts by turning the heat on or off.

Here's what's slowing down your AI strategy — and how to fix it / Image1
The ReAct model for AI agents (Credit: Confluent)

ReAct Model for AI Agents (Credit: Confluent) 

That classic definition provides a solid mental model. For today's technology, we can translate it into four key components that make up a modern AI agent:

Perception (the "senses"): This is how an agent takes in information about its digital or physical environment. It's the input stream that allows the agent to understand the current state of the world relevant to its task.

Reasoning engine (the "brain"): This is the core logic that processes the perceptions and decides what to do next. For modern agents, this is typically powered by a large language model (LLM). The engine is responsible for planning, breaking down large goals into smaller steps, handling errors and choosing the right tools for the job.

Action (the "hands"): This is how an agent affects its environment to move closer to its goal. The ability to take action via tools is what gives an agent its power.

Goal/objective: This is the overarching task or purpose that guides all of the agent's actions. It is the "why" that turns a collection of tools into a purposeful system. The goal can be simple ("Find the best price for this book") or complex ("Launch the marketing campaign for our new product")

Putting it all together, a true agent is a full-body system. The reasoning engine is the brain, but it’s useless without the senses (perception) to understand the world and the hands (actions) to change it. This complete system, all guided by a central goal, is what creates genuine agency.

With these components in mind, the distinction we made earlier becomes clear. A standard chatbot isn't a true agent. It perceives your question and acts by providing an answer, but it lacks an overarching goal and the ability to use external tools to accomplish it.

An agent, on the other hand, is software that has agency. 

It has the capacity to act independently and dynamically toward a goal. And it's this capacity that makes a discussion about the levels of autonomy so important.

Learning from the past: How we learned to classify autonomy
The dizzying pace of AI can make it feel like we're navigating uncharted territory. But when it comes to classifying autonomy, we’re not starting from scratch. Other industries have been working on this problem for decades, and their playbooks offer powerful lessons for the world of AI agents.

The core challenge is always the same: How do you create a clear, shared language for the gradual handover of responsibility from a human to a machine?

SAE levels of driving automation
Perhaps the most successful framework comes from the automotive industry. The SAE J3016 standard defines six levels of driving automation, from Level 0 (fully manual) to Level 5 (fully autonomous).

Image 2
The SAE J3016 Levels of Driving Automation (Credit: SAE International)

The SAE J3016 Levels of Driving Automation (Credit: SAE International) 

What makes this model so effective isn't its technical detail, but its focus on two simple concepts:

Dynamic driving task (DDT): This is everything involved in the real-time act of driving: steering, braking, accelerating and monitoring the road.

Operational design domain (ODD): These are the specific conditions under which the system is designed to work. For example, "only on divided highways" or "only in clear weather during the daytime."

The question for each level is simple: Who is doing the DDT, and what is the ODD? 

At Level 2, the human must supervise at all times. At Level 3, the car handles the DDT within its ODD, but the human must be ready to take over. At Level 4, the car can handle everything within its ODD, and if it encounters a problem, it can safely pull over on its own.

The key insight for AI agents: A robust framework isn't about the sophistication of the AI "brain." It's about clearly defining the division of responsibility between human and machine under specific, well-defined conditions.

Aviation's 10 Levels of Automation
While the SAE’s six levels are great for broad classification, aviation offers a more granular model for systems designed for close human-machine collaboration. The Parasuraman, Sheridan, and Wickens model proposes a detailed 10-level spectrum of automation.

image6
Levels of Automation of Decision and Action Selection for Aviation (Credit: The MITRE Corporation)

Levels of Automation of Decision and Action Selection for Aviation (Credit: The MITRE Corporation)

This framework is less about full autonomy and more about the nuances of interaction. For example:

At Level 3, the computer "narrows the selection down to a few" for the human to choose from.

At Level 6, the computer "allows the human a restricted time to veto before it executes" an action.

At Level 9, the computer "informs the human only if it, the computer, decides to."

The key insight for AI agents: This model is perfect for describing the collaborative "centaur" systems we're seeing today. Most AI agents won't be fully autonomous (Level 10) but will exist somewhere on this spectrum, acting as a co-pilot that suggests, executes with approval or acts with a veto window.

Robotics and unmanned systems
Finally, the world of robotics brings in another critical dimension: context. The National Institute of Standards and Technology's (NIST) Autonomy Levels for Unmanned Systems (ALFUS) framework was designed for systems like drones and industrial robots.

image3
The Three-Axis Model for ALFUS (Credit: NIST)

The Three-Axis Model for ALFUS (Credit: NIST) 

Its main contribution is adding context to the definition of autonomy, assessing it along three axes:

Human independence: How much human supervision is required?

Mission complexity: How difficult or unstructured is the task?

Environmental complexity: How predictable and stable is the environment in which the agent operates?

The key insight for AI agents: This framework reminds us that autonomy isn't a single number. An agent performing a simple task in a stable, predictable digital environment (like sorting files in a single folder) is fundamentally less autonomous than an agent performing a complex task across the chaotic, unpredictable environment of the open internet, even if the level of human supervision is the same.

The emerging frameworks for AI agents
Having looked at the lessons from automotive, aviation and robotics, we can now examine the emerging frameworks designed for AI agents. While the field is still new and no single standard has won out, most proposals fall into three distinct, but often overlapping, categories based on the primary question they seek to answer.

Category 1: The "What can it do?" frameworks (capability-focused)
These frameworks classify agents based on their underlying technical architecture and what they are capable of achieving. They provide a roadmap for developers, outlining a progression of increasingly sophisticated technical milestones that often correspond directly to code patterns.

A prime example of this developer-centric approach comes from Hugging Face. Their framework uses a star rating to show the gradual shift in control from human to AI:

image4
Five Levels of AI Agent Autonomy, as proposed by HuggingFace (Credit: Hugging Face)

Five Levels of AI Agent Autonomy, as proposed by HuggingFace (Credit: Hugging Face)

Zero stars (simple processor): The AI has no impact on the program's flow. It simply processes information and its output is displayed, like a print statement. The human is in complete control.

One star (router): The AI makes a basic decision that directs program flow, like choosing between two predefined paths (if/else). The human still defines how everything is done.

Two stars (tool call): The AI chooses which predefined tool to use and what arguments to use with it. The human has defined the available tools, but the AI decides how to execute them.

Three stars (multi-step agent): The AI now controls the iteration loop. It decides which tool to use, when to use it and whether to continue working on the task.

Four stars (fully autonomous): The AI can generate and execute entirely new code to accomplish a goal, going beyond the predefined tools it was given.

Strengths: This model is excellent for engineers. It's concrete, maps directly to code and clearly benchmarks the transfer of executive control to the AI. 

Weaknesses: It is highly technical and less intuitive for non-developers trying to understand an agent's real-world impact.

Category 2: The "How do we work together?" frameworks (interaction-focused)
This second category defines autonomy not by the agent’s internal skills, but by the nature of its relationship with the human user. The central question is: Who is in control, and how do we collaborate?

This approach often mirrors the nuance we saw in the aviation models. For instance, a framework detailed in the paper Levels of Autonomy for AI Agents defines levels based on the user's role:

L1 - user as an operator: The human is in direct control (like a person using Photoshop with AI-assist features).

L4 - user as an approver: The agent proposes a full plan or action, and the human must give a simple "yes" or "no" before it proceeds.

L5 - user as an observer: The agent has full autonomy to pursue a goal and simply reports its progress and results back to the human.

image2
Levels of Autonomy for AI Agents

Levels of Autonomy for AI Agents

Strengths: These frameworks are highly intuitive and user-centric. They directly address the critical issues of control, trust, and oversight.

Weaknesses: An agent with simple capabilities and one with highly advanced reasoning could both fall into the "Approver" level, so this approach can sometimes obscure the underlying technical sophistication.

Category 3: The "Who is responsible?" frameworks (governance-focused)
The final category is less concerned with how an agent works and more with what happens when it fails. These frameworks are designed to help answer crucial questions about law, safety and ethics.

Think tanks like Germany's Stiftung Neue VTrantwortung have analyzed AI agents through the lens of legal liability. Their work aims to classify agents in a way that helps regulators determine who is responsible for an agent's actions: The user who deployed it, the developer who built it or the company that owns the platform it runs on?

This perspective is essential for navigating complex regulations like the EU's Artificial Intelligence Act, which will treat AI systems differently based on the level of risk they pose.

Strengths: This approach is absolutely essential for real-world deployment. It forces the difficult but necessary conversations about accountability that build public trust.

Weaknesses: It's more of a legal or policy guide than a technical roadmap for developers.

A comprehensive understanding requires looking at all three questions at once: An agent's capabilities, how we interact with it and who is responsible for the outcome..

Identifying the gaps and challenges
Looking at the landscape of autonomy frameworks shows us that no  single model is sufficient because the true challenges lie in the gaps between them, in areas that are incredibly difficult to define and measure.

What is the "Road" for a digital agent?
The SAE framework for self-driving cars gave us the powerful concept of an ODD, the specific conditions under which a system can operate safely. For a car, that might be "divided highways, in clear weather, during the day." This is a great solution for a physical environment, but what’s the ODD for a digital agent?

The "road" for an agent is the entire internet. An infinite, chaotic and constantly changing environment. Websites get redesigned overnight, APIs are deprecated and social norms in online communities shift. 

How do we define a "safe" operational boundary for an agent that can browse websites, access databases and interact with third-party services? Answering this is one of the biggest unsolved problems. Without a clear digital ODD, we can't make the same safety guarantees that are becoming standard in the automotive world.

This is why, for now, the most effective and reliable agents operate within well-defined, closed-world scenarios. As I argued in a recent VentureBeat article, forgetting the open-world fantasies and focusing on "bounded problems" is the key to real-world success. This means defining a clear, limited set of tools, data sources and potential actions. 

Beyond simple tool use
Today's agents are getting very good at executing straightforward plans. If you tell one to "find the price of this item using Tool A, then book a meeting with Tool B," it can often succeed. But true autonomy requires much more. 

Many systems today hit a technical wall when faced with tasks that require:

Long-term reasoning and planning: Agents struggle to create and adapt complex, multi-step plans in the face of uncertainty. They can follow a recipe, but they can't yet invent one from scratch when things go wrong.

Robust self-correction: What happens when an API call fails or a website returns an unexpected error? A truly autonomous agent needs the resilience to diagnose the problem, form a new hypothesis and try a different approach, all without a human stepping in.

Composability: The future likely involves not one agent, but a team of specialized agents working together. Getting them to collaborate reliably, to pass information back and forth, delegate tasks and resolve conflicts is a monumental software engineering challenge that we are just beginning to tackle.

The elephant in the room: Alignment and control
This is the most critical challenge of all, because it's not just technical, it's deeply human. Alignment is the problem of ensuring an agent's goals and actions are consistent with our intentions and values, even when those values are complex, unstated or nuanced.

Imagine you give an agent the seemingly harmless goal of "maximizing customer engagement for our new product." The agent might correctly determine that the most effective strategy is to send a dozen notifications a day to every user. The agent has achieved its literal goal perfectly, but it has violated the unstated, common-sense goal of "don't be incredibly annoying."

This is a failure of alignment.

The core difficulty, which organizations like the AI Alignment Forum are dedicated to studying, is that it is incredibly hard to specify fuzzy, complex human preferences in the precise, literal language of code. As agents become more powerful, ensuring they are not just capable but also safe, predictable and aligned with our true intent becomes the most important challenge we face.

The future is agentic (and collaborative)
The path forward for AI agents is not a single leap to a god-like super-intelligence, but a more practical and collaborative journey. The immense challenges of open-world reasoning and perfect alignment mean that the future is a team effort.

We will see less of the single, all-powerful agent and more of an "agentic mesh" — a network of specialized agents, each operating within a bounded domain, working together to tackle complex problems. 

More importantly, they will work with us. The most valuable and safest applications will keep a human on the loop, casting them as a co-pilot or strategist to augment our intellect with the speed of machine execution. This "centaur" model will be the most effective and responsible path forward.

The frameworks we've explored aren’t just theoretical. They’re practical tools for building trust, assigning responsibility and setting clear expectations. They help developers define limits and leaders shape vision, laying the groundwork for AI to become a dependable partner in our work and lives.

Sean Falconer is Confluent's AI entrepreneur in residence.

Subscribe to get latest news!
Deep insights for enterprise AI, data, and security leaders

VB Daily
AI Weekly
AGI Weekly
Security Weekly
Data Infrastructure Weekly
VB Events
All of them
Enter Your Email

Get updates

More
Press Releases
Contact Us
Advertise
Share a News Tip
Contribute
Privacy Policy
Terms of Service
Do not sell my personal info
© 2025 VentureBeat. All rights reserved.

