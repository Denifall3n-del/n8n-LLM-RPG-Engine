# Autonomous LLM RPG Engine (n8n + PostgreSQL)

An event-driven **n8n workflow** that runs a fully autonomous, stateful text-based RPG through a Telegram bot. Built as a practical exercise in the harder parts of working with LLMs: keeping them in sync with a database, forcing structured output, managing context windows over long sessions, and recovering gracefully when the model returns something broken.

## Architecture Overview

The system is more than a prompt-response loop. It's a backend engine that handles database reads and writes, context window optimization, and logic routing before and after each LLM call.

![n8n Workflow Schema](Screenshot%202026-07-03%20132351.png)

## Key Technical Features

- **Multi-agent orchestration.** Distinct prompts and logic chains for different jobs. One branch generates the narrative turn; a second evaluation branch reads that narrative and extracts world entities (NPCs, contracts, factions) as structured data.

- **Strict JSON extraction.** The LLM is constrained to return validated JSON, which is parsed and written into PostgreSQL to persist world state across sessions. Malformed responses are caught before they touch the database.

- **State and memory management (PostgreSQL).** Tracks sessions, user IDs, and turn counts. Dynamically assembles each prompt from a World Bible, recent turns, and pinned "important" memories, pruning older context to stay within model limits.

- **Fault tolerance and handoff.** Data validation on every LLM output, error branches for empty or malformed API responses, and automatic session reset paths. The workflow is documented so someone else can pick it up and run it without me sitting next to them.

## What I'd do differently

A few things I'd change on a rewrite, in rough order of importance:

- **Split the monolith.** Right now everything lives in one large n8n workflow. Narrative generation and entity extraction should be two separate workflows communicating over a queue — easier to debug, easier to swap models per stage, and it stops one failure from blocking the whole turn.
- **Better memory scoring.** Old memories are pruned by recency and a simple importance flag. A proper embedding-based relevance score would keep the prompt payload smaller and more useful, especially in long campaigns.
- **Semantic validation, not just structural.** JSON validation catches malformed output but not hallucinated entities (an NPC the model invented that contradicts the World Bible). A second lightweight check against known entities would catch this.
- **Cost visibility.** No per-session cost tracking yet. On long sessions the context payload grows and gets expensive; I'd add token accounting to the database so it's visible per user.

## Why I built it

Wanted to see whether I could make an LLM behave like a real backend component — constrained, persistent, and integrated with a normal relational database — rather than a chatbot that forgets everything. Most of the interesting problems ended up being about state management and error handling, not prompting.

