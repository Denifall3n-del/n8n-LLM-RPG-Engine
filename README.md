<h1>Autonomous LLM RPG Engine (n8n and PostgreSQL)</h1>
<p>This repository contains an advanced, event-driven <b>n8n workflow</b> that orchestrates a fully autonomous, stateful text-based RPG via Telegram. It demonstrates complex LLM orchestration, structured data extraction, and long-term memory management.</p>
<h2>Architecture Overview</h2>
<p>The system goes far beyond simple prompt-response mechanics. It acts as a full backend engine handling database operations, context window optimization, and logic routing before and after the LLM generates a response.</p>
<p><img src="Screenshot 2026-07-03 132351.png" alt="n8n Workflow Schema" width="100%"></p>
<h2>Key Technical Features</h2>
<ul>
<li><b>Multi-Agent Orchestration:</b> The workflow utilizes different prompts and logic chains for distinct tasks. One branch generates the narrative, while a secondary evaluation branch extracts specific world entities (NPCs, Contracts, Factions) from the narrative text.</li>
<li><b>Strict JSON Data Extraction:</b> The engine forces the LLM to analyze the generated story turn and output structured JSON, which is then parsed and injected into the PostgreSQL database to permanently update the world state.</li>
<li><b>State and Memory Management (PostgreSQL):</b> Tracks sessions, user IDs, and turn counts. It dynamically loads a World Bible, recent turns, and important memories to build an optimized prompt payload, pruning old memories to strictly manage the LLM context window limits.</li>
<li><b>Fault Tolerance and Handoff:</b> Includes strict data validation, error handling for empty API responses, and session resets, ensuring the system remains stable and production-ready without constant developer supervision.</li>
</ul>
<h2>Why this matters</h2>
<p>This project highlights a practical, utility player approach to AI automation. It showcases the ability to scope a complex problem, build a robust node-based solution, and ensure the AI remains constrained, useful, and integrated with traditional data structures.</p>
</pre>
