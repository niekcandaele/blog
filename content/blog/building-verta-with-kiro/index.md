---
title: "Building Verta: How Kiro's Spec-Driven Development Finally Made AI Coding Work"
date: "2025-09-15T07:26:03.284Z"
categories: [code]
---

## The Problem: Discord is a Black Hole for Information

I've been frustrated with Discord for years. It's where communities live, but it's also where information goes to die. You have an insightful conversation, someone asks a great question, you provide a detailed answer—and within days, it's buried under thousands of new messages, gone forever.

This hit me particularly hard with [CSMM](https://csmm.app/) and [Takaro](https://takaro.io/), where our community kept needing answers to the same questions over and over. Knowledge wasn't accumulating; it was evaporating. So naturally, I wanted to build something to fix this—a system that could capture Discord conversations, make them searchable, and turn community knowledge into something permanent and useful.

I tried building a solution with AI twice before, the kids call it "vibe coding"—just me and an AI assistant trying to build something functional. Both attempts failed spectacularly (exhibit [1](https://github.com/niekcandaele/ai-discord-support) and [2](https://github.com/niekcandaele/sapha)), and looking back, it's obvious why. This isn't some simple CRUD app you can bang out in an afternoon. It's a genuinely complex system with a frontend for browsing the knowledge base, a backend API handling Discord actions and orchestration, a machine learning service for embeddings and search, a database with vector search capabilities, and background workers for syncing millions of messages.

Getting all these components to work together while keeping the bigger picture in mind? That's where pure AI coding falls apart. The context windows are too small, the AI forgets what it's building, and you end up with a mess of half-implemented features that don't quite connect. Then I found Kiro, and everything changed.

## Spec-Driven Development: Why It Actually Works This Time

I'd seen spec-driven development approaches before—there are projects like [Taskmaster AI](https://www.task-master.dev/) or [ai-dev-tasks](https://github.com/snarktank/ai-dev-tasks) that promise structured AI workflows. But they all fall victim to what I call the "LLM barf problem." Ask an LLM to plan a feature, and it'll eagerly vomit thousands of characters at you—planning phases, rollback strategies, testing strategies, performance goals, architectural diagrams, the works. It looks impressive at first glance, but it's actually useless fluff that drowns out what you're trying to build.

Kiro is different because it keeps specs lightweight and focused. This was the game changer for me—instead of drowning in documentation, I could actually see what I was building. The workflow forced me to think differently: I'd write a spec describing what I want, Kiro would create a design proposal, and then I'd review it and immediately see issues. "This won't work," I'd think, or "I wasn't specific enough about that requirement." We'd iterate together until the spec was solid, and only then would implementation begin.

This back-and-forth before any code is written? That's what made the difference. With Kiro, the AI always has something concrete to reference—the spec—so it can't drift off into implementing random features or forgetting the architecture halfway through. It's like having guardrails that keep the AI focused on what actually matters.

## Technical Highlight: The Search Endpoint Evolution

Let me give you a concrete example of how this worked in practice. The search endpoint is the heart of Verta—when someone asks a question in Discord, the bot uses search to find relevant past discussions. The frontend uses it for browsing, question clustering depends on it, basically everything revolves around search working well.

My first implementation was, to put it mildly, garbage. Basic keyword matching that missed most relevant results and returned a lot of noise. I knew it needed to be better but wasn't sure how to approach it, so I asked Kiro: "This search isn't performing well. Can you help me figure out better approaches?"

What Kiro suggested was actually quite pragmatic. First, do an initial search to get preliminary results. Then feed those results to an LLM and ask: "Based on these results, what other searches might find relevant information?" Execute those additional searches, then combine everything for the final answer. If you were a crazed marketing person, you might call this a "multi-step intelligent AI agent", I call it a pragmatic little function. Simple enough to implement quickly, sophisticated enough to dramatically improve result quality. That's the kind of suggestion that makes Kiro valuable: not overengineering, just solving the problem at hand.

## Lessons Learned: The Reality of AI-Assisted Development

My workflow has completely shifted since adopting this approach. Before, I spent most of my time in my editor—writing code, reading code, debugging code, the usual developer grind. Now I spend most of my time in markdown files, crafting and refining specs. It's a different kind of work, more about thinking through problems than implementing solutions.

The default Kiro specs are vanilla, which makes sense—they need to cater to a generic audience. But I've been writing software for years and I know how I want things built, so I customize heavily. I delete the fluff I don't need, add precise descriptions of what I want, and iterate multiple times before implementation. This isn't a bug; it's the reality of working with AI tools. No AI generates perfect specs in one shot, and that's fine. The magic is in the iteration process.

Here's the crucial part that a lot of AI tool marketing glosses over: you can't trust any AI agent completely. They're all flawed in their own ways, and you need to carefully review every piece of generated code, test thoroughly, and be ready to intervene when they go off track. It's not about replacing human judgment; it's about augmenting it.

Collaboration is really the right word here. I use my human brain to guide the AI through the specs, and the AI handles the implementation details. Neither of us could build Verta alone—I'd get bogged down in implementation details, and the AI would build something that technically works but misses the point.

## Conclusion

Verta exists because Kiro made spec-driven development practical—not the bloated, over-engineered version I'd seen before, but a lightweight approach that actually helps you build things. The shift from "writing code all day" to "thinking in specs" isn't just about productivity; it's about building better software by thinking through problems before implementing solutions.

Is it perfect? No. Do I still need to babysit the AI and catch its mistakes? Absolutely. But for the first time, I have a working Discord knowledge management system that's actually in production, serving real users, and solving the problem I set out to fix. After two failed attempts, that feels pretty good.

Sometimes that's all that matters—not having the perfect process or the most elegant code, but actually shipping something that solves a real problem. And if it takes a combination of human creativity and AI implementation to get there? I'll take it.
