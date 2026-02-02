# Agent Engineering is Here: Why PMs Who Can Build Will Win

*The role of Product Manager is splitting in two. Which side are you on?*

## Introduction

A new discipline is forming in tech, and Product Managers should pay attention: the gap between people who can ship and people who can only write specs grows wider every month.

LangChain recently published "Agent Engineering: A New Discipline," describing the iterative process of turning non-deterministic LLM systems into reliable production experiences. Build, test, ship, observe, refine, repeat. But there's something more significant for PMs buried in that piece: the acknowledgment that agent engineering sits at the intersection of product thinking, engineering, and data science.

One line caught my attention: *"Individuals that can excel in multiple of these fields will bring exponential value to their work and organizations."*

That's not a prediction about the future. It's happening now. And it's why I'm learning to build with AI through AI Makerspace's AI Engineering Bootcamp.

## Prerequisites

You don't need much to follow along:

- Familiarity with what LLMs can do (you probably use ChatGPT or Claude already)
- Curiosity about how AI applications work under the hood
- A willingness to question whether "waiting for engineering" is the only option


## From Prompt Engineering to Agent Engineering

The industry has moved through three phases in rapid succession:

**Prompt Engineering** was about optimizing a single prompt for accuracy, determinism, and cost efficiency. You could pick this up in an afternoon.

**Context Engineering** widened the scope. Now you're optimizing everything in the LLM's context window: the prompt, referenced documents, tool calls, context length. You need to understand how retrieval systems work and how different pieces of information interact with each other.

**Agent Engineering** goes further. You're refining entire non-deterministic systems into reliable production experiences. An agent doesn't just respond to a prompt—it reasons, calls tools, and makes decisions across multiple steps.

The key difference: traditional software assumes you mostly know the inputs and can define the outputs. Agents give you neither. Users can say anything, and because LLMs are non-deterministic, the range of possible behaviors is wide open.

This is why the old approach—perfect something before shipping—doesn't work anymore.

## "Ship First, Learn in Production"

LangChain makes a claim that sounds odd at first: *"Shipping isn't the end goal. It's just the way you keep moving to get new insights and improve your agent."*

It's pragmatism. When every input is effectively an edge case—when users can type "make it pop" or "do what you did last time but differently"—you can't test exhaustively before launch. Real-world behavior will surprise you.

Teams shipping reliable agents today have stopped trying to perfect things before launch. Production is their teacher.

For Product Managers, this should sound familiar. It's lean product development taken to its logical conclusion. But if you can't build the thing yourself, you're still dependent on someone else to ship those rapid iterations. You're still in "spec and wait" mode, even if your specs are better.

## Vibe Checking: The PM's Entry Point

Before you build sophisticated evaluation pipelines, you need to understand what "working" even means for an AI system. Harder than it sounds.

An agent can have 99.99% uptime while being completely off the rails. Simple yes/no answers don't always exist for the questions that matter: Is the agent making the right calls? Using tools the right way? Following the intent behind your instructions?

"Vibe checking" is the informal term for cursory, unstructured, non-comprehensive evaluation of LLM-powered systems. The goal is to loosely evaluate your application to cover significant functions where failure would be immediately noticeable and severe.

In practice:

1. **General capability checks**: Can the system explain concepts? Summarize text? Generate creative content? Handle basic reasoning?

2. **Domain-specific checks**: Can it help with decisions you'd actually ask it about? Draft communications you'd actually send?

3. **Capability boundary checks**: What happens when you ask for things that require external tools, real-time data, or actions the system can't take?

The goal is understanding where your system succeeds, where it fails, and where the boundaries are. A PM can develop this understanding directly: you don't need engineering to run prompts and evaluate outputs.

## What This Means for Product Managers

LangChain defines what each role contributes:

**Product Thinking:**
- Writing prompts that drive agent behavior
- Deep understanding of the job to be done that the agent replicates
- Defining evaluations that test whether the agent performs as intended

**Engineering:**
- Writing tools for agents to use
- Developing UI/UX for agent interactions
- Creating reliable runtimes for durable execution

**Data Science:**
- Building systems to measure agent performance
- Analyzing usage patterns and errors
- Running evals over production data

Look at the product thinking list again. Writing prompts, understanding jobs to be done, defining evaluations; PMs can do these directly, if they know how.

Engineering is where vibe coding comes in. With AI-assisted development, a PM can build tools, create interfaces, and ship working prototypes. The barrier to building has dropped dramatically.

## Even Simple Evals Matter

One of the pre-reads for this session was Shreya Shankar's "In Defense of AI Evals, for Everyone." Her argument: every successful product runs evals somewhere in the lifecycle. The question isn't whether to use evals, but how rigorous they need to be.

You can be less rigorous if:
- Your task is already well-represented in the model's training (like basic coding)
- Your team has deep enough domain expertise to test the product as users, steering by feel

For everyone else, and that includes most PM-led prototypes, you need some form of evaluation. Vibe checking is the minimum viable version. It won't catch everything, but it establishes a baseline and reveals obvious failures.

Foundation model providers pour enormous resources into evaluation for every new capability area. When you build on top of those models, you inherit some of that work. But you also introduce your own application-specific ways to fail. Only you can test for those.

## Key Takeaways

1. **Agent engineering combines product thinking, engineering, and data science.** PMs who can operate across these areas will bring exponential value. Those who can't will increasingly be left behind.

2. **"Ship first, learn in production" isn't optional for AI products.** When every input is an edge case and behaviors are non-deterministic, you can't perfect things before launch. You have to observe and refine in real-world conditions.

3. **Vibe checking is accessible to PMs.** You don't need engineering support to run prompts, evaluate outputs, and identify where your system succeeds or fails. This is hands-on product work.

4. **The barrier to building has dropped.** With AI-assisted development, PMs can write tools, create interfaces, and ship working prototypes. It's about becoming complete—bundling product thinking with technical execution.

5. **Evals matter, even simple ones.** A baseline vibe check reveals obvious failures and helps you understand your system's capabilities and boundaries before users do.

## Conclusion

The PM role is forking. One path leads to deeper specialization in strategy, research, and stakeholder management: valuable work, but increasingly disconnected from actual building. The other path leads to Product Engineering: combining business skills with technical execution, shipping prototypes, validating ideas by building rather than speccing.

LangChain puts it plainly: *"Agent engineering is not a new job title. Rather, it's a set of tasks and responsibilities that existing team members, across different disciplines, have to take on."*

That's an invitation. PMs can take on these responsibilities, or watch as engineers and more technically-inclined product people fill the gap.

Vibe coding is the bridge. It's what lets you go from "I understand what needs to be built" to "I built it, tested it, and here's what I learned." And that capability will only become more powerful as the tools improve.

The only question left: how quickly can you start?

## Resources

- [Agent Engineering: A New Discipline](https://www.blog.langchain.com/agent-engineering-a-new-discipline/) - LangChain's breakdown of the emerging discipline
- [In Defense of AI Evals, for Everyone](https://www.sh-reya.com/blog/in-defense-ai-evals/) - Shreya Shankar on why evals matter
- [AI Makerspace Bootcamp](https://aimakerspace.io/) - Where I'm learning AI engineering
- [Language Models are Few-Shot Learners (2020)](https://arxiv.org/abs/2005.14165) - The foundation of in-context learning
- [Chain-of-Thought Prompting (2022)](https://arxiv.org/abs/2201.11903) - How to improve LLM reasoning

---
*This post is part of my journey learning to build with AI through AI Makerspace's AI Engineering Bootcamp (AIE9).*
