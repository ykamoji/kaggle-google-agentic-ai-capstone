## Project Overview: Microsoft OneNote Agent Organizer

This project contains the core logic for the Microsoft OneNote Agent, a multi-agent system designed to help users organize, navigate, and view all their notebooks, sections, pages, and content in a structured and intelligent way.

The agent is built using the Google Agent Development Kit (ADK), leveraging a combination of multi-agent environments, custom OpenAPI tools, and robust session/state management to deliver context-aware interactions.

#### Features

- Multi-agent environment enabling coordinated task execution
- Custom OpenAPI tools (Microsoft Graph API) for interacting with OneNote data structures
- Session and state management for maintaining conversational and operational context
- Intelligent navigation across notebooks, sections, and pages.

<hr/>

### Problem Statement

Modern digital note-taking platforms like Microsoft OneNote organize information in deeply nested hierarchies **notebooks** → **sections** → **pages** → **content**. 

Navigating this structure efficiently can be challenging for users, especially when trying to retrieve specific information across multiple levels. While OneNote exposes APIs for accessing these resources, working with them directly involves several complexities, including authentication workflows, understanding hierarchical relationships, and performing multiple dependent API calls in the correct sequence.

At the same time, standard LLM-based agents are not naturally suited for long-term, multi-step retrieval tasks. Without explicit state management and orchestration, they tend to lose context between steps, repeat the same queries, or return incomplete or inconsistent results.

<hr/>

### Solution Statement

This project addresses these challenges by building a multi-agent, context-aware orchestration agents using the Google Agent Development Kit (ADK).
The goal is to enable users to query their OneNote workspace naturally—such as asking for notebooks, sections, pages, or specific page content—while the agent automatically:

- Identifies the type of user request (notebook, section, page, or content).
- Delegates the task to the correct specialized agent.
- Fetches required data through a controlled, read-only OneNote API agent.
- Persists retrieved IDs and metadata (notebook, section, page) in session state.
- Avoids redundant API calls by reusing previously stored session information.
- Returns a structured and reliable response back to the user.

The Agent enforces strict rules such as:

- Only allowing access to the "Google ADK" notebook.
- Ensuring response formatting consistency. 
- Ensuring that every orchestrator agent completes memory-saving steps before concluding its turn.

By combining multi-agent workflows, custom OpenAPI tools, and session/state management, this project demonstrates how LLM agents can be transformed into stateful, deterministic, and reliable assistants for navigating hierarchical data systems like OneNote.

<hr/>

### Architecture

This Agents are designed to retrieve hierarchical OneNote data while preserving context across user interactions.


![Architecture Flowchart](https://raw.githubusercontent.com/ykamoji/kaggle-google-agentic-ai-capstone/main/Architecture.png)

### 1. Root Orchestrator (Delegation Layer)
The top-level `OneNoteAgent` analyzes user intent and routes the query to the appropriate orchestrator:
- Notebook queries → `Notenook Agent`
- Section queries → `Section Agent`
- Page queries → `Page Agent`
- Content queries → `Content Agent`

It returns the result from these agent tools.

### 2. Agents Tool (Logic Layer)
Four agent tools coordinate how notebook, section, page, and content data are fetched

Each orchestrator follows a strict execution plan:
1. Check stored memory using its corresponding `retrieve_*` tool.
2. If missing, call `OneNote API Agent` to fetch data.
3. Parse the returned output.
4. Save the extracted IDs and names using `save_*` tools.
5. Respond to the **user** with the final result.

Ensures deterministic behavior, consistent retrieval, and efficient reuse of previously fetched data.

### 3. Memory Tools (Session State Layer)
Custom tools are used to store and retrieve hierarchical metadata across the session:
- Notebook memory: `save_notebookinfo`, `retrieve_notebookinfo`
- Section memory: `save_sectionsinfo`, `retrieve_sectionsinfo`
- Page memory: `save_pageinfo`, `retrieve_pageinfo`
- Content memory: `save_content`, `retrieve_content`

Each tool stores structured information inside the session, preventing repeated API calls and enabling long-term context.


### 4. OneNote API Agent (Data Retrieval Layer)
The `oneNoteAPIAgent` serves as the read-only interface to the OneNote API. It uses:
- A custom OpenAPI toolset (from `onenote_read_spec`) for Microsoft GRAPH Api (with Auth).
- Strict instructions for calling only: `list_notebooks`, `list_sections`, `list_pages`, and `get_page_content`
- A restriction to operate only on the "Google ADK" notebook (Additional Security)
- A required output format for notebook discovery

This agent performs all external API calls and ensures controlled, predictable data retrieval.

#### Execution & Session Management
- `InMemorySessionService` for session persistence
- `Runner` for executing the root agent within a tracked session

This enables stateful multi-step interactions where previously retrieved notebook, section, page, and content data are remembered.

<hr/>

### Conclusion
This project demonstrates how multi-agent orchestration, custom OpenAPI tooling, and persistent session state can transform a complex hierarchical system like Microsoft OneNote into an intuitive, conversational interface. By combining specialized agents with a shared memory layer, the Agents delivers reliable, context-aware retrieval of notebooks, sections, pages, and content—without redundant API calls or loss of state.

Overall, my work highlights the strength of agentic designs built with Google ADK, showing how structured tooling and coordinated agents can create **mostly** (since I didn't add safeguards on the user output format, it deviates every time) deterministic, and user-friendly LLM-powered applications.

Future implementations involve tasks like parallel summarization for each of the pages in the sections using ParallelAgents.

<hr/>

### Value Statement

Being an immersive writer, I rely heavily on OneNote for drafting, organizing ideas, and tracking multiple storylines. Over time, my workspace became crowded with countless notebooks, sections, and pages—often making it difficult to locate the exact content I needed. Now, instead of manually scrolling and searching, I can simply ask the LLM Agent to get the answer instantly, without need to hunt for that specific notebook or page.

Building this agentic system has dramatically improved the way I work with OneNote. With natural, conversational prompts, I can retrieve any notebook, section, page, or content on demand. The agent automatically maintains context, remembers previously accessed information, which saves me valuable time during writing, outlining, and research.
