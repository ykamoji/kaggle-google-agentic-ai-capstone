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

### Solution Statement

This project addresses these challenges by building a multi-agent, context-aware orchestration system using the Google Agent Development Kit (ADK).
The goal is to enable users to query their OneNote workspace naturally—such as asking for notebooks, sections, pages, or specific page content—while the agent system automatically:

- Identifies the type of user request (notebook, section, page, or content).
- Delegates the task to the correct specialized agent.
- Fetches required data through a controlled, read-only OneNote API agent.
- Persists retrieved IDs and metadata (notebook, section, page) in session state.
- Avoids redundant API calls by reusing previously stored session information.
- Returns a structured and reliable response back to the user.

The system enforces strict rules such as:

- Only allowing access to the "Google ADK" notebook.
- Ensuring response formatting consistency. 
- Ensuring that every orchestrator agent completes memory-saving steps before concluding its turn.

By combining multi-agent workflows, custom OpenAPI tools, and session/state management, this project demonstrates how LLM agents can be transformed into stateful, deterministic, and reliable assistants for navigating hierarchical data systems like OneNote.





### Architecture


### Essential Tools and Utilities


### Conclusion



### Value Statement


## Workflow
