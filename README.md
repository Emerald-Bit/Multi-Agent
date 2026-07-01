# Multi-Agent Research System

<video src="https://github.com/user-attachments/assets/3fada51f-6d1a-4b6e-876a-aa5bea5b7691" controls width="700"></video>

**The Problem**

SMEs often rely on scattered knowledge across documents, websites, notes, emails, CRM exports, staff memory, and ad hoc research. This created a familiar problem: valuable information existed, but it was slow to find, hard to trust, and easy to lose when people moved between projects or teams.

For smaller teams, this became a productivity bottleneck. Staff spent too much time repeating research, checking sources manually, rewriting similar outputs, and trying to remember previous decisions. Generic chatbots helped with surface-level answers, but they did not reliably follow company context, preserve feedback, or provide controlled workflows for higher-stakes business research.

**The Solution**

To address this, I built and deployed a multi-agent research assistant tailored for SME workflows using LangGraph and Retrieval-Augmented Generation (RAG). The system successfully turned rough business questions into structured research workflows: it optimised the request, loaded relevant company context, retrieved prior lessons, created a plan, paused for human approval, executed the work with tools, reviewed the output, and asked for final human sign-off before saving reusable feedback.

The solution provided a practical internal research layer that continuously improved over time, while keeping humans firmly in control of planning and final output quality. The architecture included dedicated optimiser, planner, executor, reviewer, human review, skill-loading, context-loading, and feedback extraction nodes.

**Core Tech Stack**

* **AI / Agent Orchestration:** LangGraph, LangChain, OpenAI, Anthropic, Google Gemini, Ollama/Qwen support.
* **RAG / Memory:** Chroma, OpenAI text-embedding-3-small, SQLRecordManager, Flashrank reranking, MMR retrieval, SQLite checkpointing, Neon/Postgres long-term store.
* **Tools / Data Access:** Exa web search, Firecrawl scraping, Hunter.io contact discovery, calculator, date/time utilities, internal knowledge RAG, feedback-lessons RAG.
* **App / Interaction Layer:** Python application with LangGraph interrupts for human-in-the-loop review.
* **Reliability / Control:** Pydantic structured outputs, typed LangGraph state, tool-call limits, model-call limits, retry middleware, summarisation middleware, recursion limits, human approval gates.




**Key Features & Engineering Justifications**

* **Human-in-the-Loop Workflow:** The assistant did not blindly execute user requests. It created a plan and success criteria, paused for human approval, executed only after approval, and then routed the output through reviewer and final human review stages. This eliminated the risk of low-quality or off-brand AI outputs being used directly.
* **Multi-Agent Planning and Review:** The system separated planning, execution, and review into distinct nodes. This made the workflow significantly easier to debug, evaluate, and scale compared to a single all-purpose chatbot. The reviewer checked executor output against approved success criteria before it ever reached the user.
* **Company Context and Skill Loading:** The agent loaded identity/context files and scanned a skills directory for available procedural skills. This allowed the SME to easily encode repeatable workflows, brand preferences, research methods, and business-specific instructions without hard-coding everything into one rigid prompt.
* **RAG-Based Internal Knowledge Retrieval:** The project utilised Chroma with OpenAI embeddings to index and retrieve internal and scraped knowledge. Documents were chunked, tagged, stored with stable metadata, retrieved with MMR, and reranked with Flashrank. This grounded the assistant's answers in actual business material rather than relying on model hallucinations.
* **Feedback Lessons Memory:** After a completed run, the assistant systematically extracted reusable lessons from reviewer and human feedback. This ensured the system learned and adapted over time by remembering repeated corrections, preferred output styles, project-specific decisions, and recurring failure patterns.
* **Operational Guardrails:** The execution agent included model-call limits, tool-call limits, retry middleware, summarisation middleware, and recursion limits. For the SME, this was critical because it prevented uncontrolled agent loops from burning API budgets, stopped overly aggressive web scraping, and ensured clean, concise outputs.
* **External Research and Lead Discovery Tools:** The assistant searched the web with Exa, scraped pages with Firecrawl, and leveraged Hunter.io for domain and contact discovery. This automated high-value SME use cases like competitor research, lead sourcing, market mapping, supplier research, and proposal preparation.

**Additional Demo Images**

The graph:
<img width="1150" height="1261" alt="Image" src="https://github.com/user-attachments/assets/bb10eb59-4ed4-4afe-867d-e70a4cf5b66e" />

Demo Output and Learnings to be sent to the lessons learned RAG (for continous self improvement)
<img width="1251" height="1080" alt="Image" src="https://github.com/user-attachments/assets/ba19c376-c729-46dd-8223-17d89667c643" />
