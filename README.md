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


##Orchestration pattern choice and justification 
Requests vary (plan vs risk vs status vs milestone), so a router pattern is more suitable for this use case. Router agent will classify each request and accordingly dispatch the request to specialists agent — each grounded in the ABCDE Ltd. 

## Agent Definition

Orchestrator Agent : Classify the request type as plan request if the request is similar to generate a project plan and routes the request to Planner agent. If the request type is for assessing project risk, then route the request to Risk Assessor agent. If the request is for getting the project status, router the request to Status Reporter agent.

Planner Agent: Ingests project description and timeline, generates structured project plan with phases and milestones.

Risk Assessor Agent: Analyzes project details, generates categorized risk matrix with mitigations.

Status Reporter Agent: Reads task board, generates formatted weekly status report and blockers.

Milestone Tracker Agent: Compares timeline vs current progress, flags at-risk milestones.

## 🛠️ Agent Directory & Specifications

| Agent | Trigger Condition (Orchestrator Routing) | Required Context Inputs | Deliverable / Output Schema |
| :--- | :--- | :--- | :--- |
| **🤖 Orchestrator** | Initial gateway for all user queries. | Raw user prompt. | Categorized router payload mapping to a downstream agent. |
| **📋 Planner** | Matches user intent to generate or modify a project plan. | Project descriptions, core timelines, baseline schedules. | Structured roadmap complete with clear project phases and milestones. |
| **⚠️ Risk Assessor** | Matches intent to identify threats, gaps, or evaluate risk profiles. | High-level project specifications and operational timelines. | Categorized risk assessment matrix equipped with actionable mitigation steps. |
| **📊 Status Reporter** | Matches intent seeking active progress tracking or high-level status updates. | Operational task boards, live project backlog, sprint metrics. | Formatted weekly status summary highlighting work completed and active blockers. |
| **🎯 Milestone Tracker**| Chains downstream from the Status Reporter tool or timeline changes. | Baselined calendar schedule vs. current task progression. | High-priority exception alert flagging at-risk milestones. |


# 🔀 Agentic Workflow System Architecture

This document maps out the multi-agent routing workflow, visualizes the decision matrix, and details the data inputs and deliverables for each specialized agent node.

## 📊 System Diagram

<img width="783" height="958" alt="Screenshot 2026-09-10 at 1 37 50 PM" src="https://github.com/user-attachments/assets/24de29d7-99c6-4932-b1aa-2e7b97048b10" />
