# capstone_mira
AI assistant that generates structured project plans from high-level descriptions, produces categorized risk assessments, and compiles weekly status reports from task board data.
I choose this project, as it relates/align more on my current job task and future roles that I am looking into.

## Architecture Writeup

## Problem Summary
Nexora's PMs/TPMs are overwhelmed by manual administrative tasks, spending hours creating project plans from high-level descriptions, tracking risks inconsistently, and compiling weekly status reports. To solve this, the Mira AI assistant will automate the generation of these standardized project artifacts from raw data to eliminate operational inefficiencies and enable proactive milestone alerts. The biggest risk is that the AI might hallucinate requirements or omit critical dependencies, leading to wasted engineering effort, missed project deadlines, or flawed deployment strategies for clients like ABCDE Ltd.

## 🏗️ System Architecture

| Category | Component / Specification | Description & Architectural Choice |
| :--- | :--- | :--- |
| **Workflow Platform** | **Langflow** | Serves as the **visual orchestration canvas** to design, test, and execute the multi-agent workflows. It connects data ingestion components, routers, and specialized agents into a unified executable graph. |
| **AI/ML Layer (Routing)** | **gpt-4o-mini** | Deployed as the primary **Router Agent**. Selected for its low cost and high speed, it serves as the fast classification and extraction engine to parse user intents and forward tasks to the correct downstream agent. |
| **AI/ML Layer (Specialized)** | **gpt-4o / GPT-4 family** | Powers the **Specialized Reasoner Agents**. These models handle heavy-duty reasoning, multi-step project planning, domain-specific logic, and high-quality narrative report generation. |
| **LLM Observability** | **Langfuse** | Integrated directly into the **Langflow engine** to provide full production tracing. Tracks agent execution paths, step-by-step token costs, and latency bottlenecks to simplify debugging. |
| **Data & Integration** | **Context Ingestion** | Input interfaces mapped to ingest unstructured text data (meeting notes, raw task board updates) into the Langflow canvas for processing by extraction nodes. |
