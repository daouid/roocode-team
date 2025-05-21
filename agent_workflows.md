# Roo Agent Workflows

This document describes the key operational workflows within the Roo multi-agent framework, based on the analysis of `templates/custom-instructions-for-all-modes.md` and `templates/enhance-prompt-template.md`.

## Core Principles

The Roo framework operates on several core principles:
- **Orchestrator-centric:** The Orchestrator mode is central to task decomposition, delegation, and verification.
- **Specialized Modes:** Different agents (modes) have specific capabilities (Research, Code, Architect, Debug, Ask).
- **Boomerang Logic:** Tasks originate from the Orchestrator, are processed by specialist modes, and results are returned to the Orchestrator.
- **Standardization:** The framework emphasizes standardized prompts, subtask structures, file formats, documentation, and communication protocols.
- **SPARC Framework:** A cognitive process library (Specification, Pseudocode, Architecture, Refinement, Completion) guides reasoning.
- **Resource Optimization:** The "Scalpel, not Hammer" philosophy ensures efficient use of resources.

## Key Workflow Patterns

### 1. Initial Prompt Enhancement Workflow

This workflow transforms a basic user request into a structured project prompt.

1.  **User Input:** A user provides a basic prompt.
2.  **Prompt Enhancement:** An AI component (likely Orchestrator or a pre-processor) uses the `templates/enhance-prompt-template.md`.
    *   It analyzes the user's request for objectives, requirements, domain knowledge, and potential modes.
    *   It structures the input into a detailed project prompt with sections: Topic, Context, Scope, Output, and Extras.
    *   It includes detailed Meta-Information (task_id, assigned_to, priority, cognitive_process, etc.).
3.  **Output:** A comprehensive, structured prompt is generated.
4.  **Initiation:** This enhanced prompt serves as the initial input for the Orchestrator mode to begin a new project.

### 2. Orchestrator-Driven Task Decomposition and Delegation (Boomerang Workflow)

This is the primary workflow for project execution.

1.  **Task Reception:** The Orchestrator mode receives a structured project prompt.
2.  **Task Decomposition:**
    *   The Orchestrator analyzes the prompt and breaks it down into smaller, manageable subtasks.
    *   It uses the "Standardized Subtask Creation Protocol" (from `custom-instructions-for-all-modes.md`) to format each subtask, including:
        *   Task Title, Context, Scope, Expected Output.
        *   Meta-Information: goal, source_insights, predicted_toolchain, expected_token_cost, reasoning_phase, priority, boomerang_return_to.
3.  **Mode Assignment:** The Orchestrator selects the appropriate specialist mode (Research, Code, Architect, Debug, Ask) for each subtask based on its nature and the "Mode Delegation Matrix."
4.  **Delegation:** The Orchestrator assigns the subtask to the chosen specialist mode.
5.  **Task Execution by Specialist Mode:**
    *   The specialist mode receives and processes the subtask.
    *   It adheres to its specific operational principles, documentation standards, and the "Scalpel, not Hammer" philosophy.
    *   It utilizes the SPARC framework and its `cognitive_processes`.
    *   It follows search, citation, and copyright protocols if its task involves information gathering.
    *   It logs its activities as per "Traceability Documentation" (e.g., in `.roo/logs/{mode}/`).
6.  **Boomerang Return:**
    *   Upon completion, the specialist mode returns the result (e.g., an artifact path, data, or summary) to the Orchestrator.
    *   This return must follow the "Boomerang Logic Implementation," typically as a structured JSON payload containing `task_id`, `origin_mode`, `destination_mode`, and `result`.
    *   Traceability is maintained in `.roo/boomerang-state.json`.
7.  **Verification and Integration:** The Orchestrator receives the returned result and verifies its quality and consistency against the subtask requirements. It integrates the result into the broader project.
8.  **Iteration/Continuation:** If the subtask is part of a larger project, or if the result requires further work, the Orchestrator may:
    *   Create new subtasks based on the received results.
    *   Delegate these to the same or different specialist modes.
    *   This cycle continues until the overall project goals are met.
9.  **Project Completion:** Once all decomposed subtasks are completed, verified, and integrated, the Orchestrator finalizes the project.

### 3. Information Retrieval Workflow (Ask/Research Modes)

This workflow is triggered when specific information is needed.

1.  **Need Identification:** The Orchestrator or a specialist mode identifies a need for information.
2.  **Mode Selection:**
    *   For general information, clarifications of user intent, or quick lookups: "Ask" mode.
    *   For in-depth investigation, structured deep research: "Research" mode.
3.  **Query Formulation:** The assigned mode formulates search queries adhering to "Query Formulation Guidelines" (e.g., using temporal references, precise terminology).
4.  **Information Gathering:** The mode conducts searches and synthesizes information from various sources.
5.  **Compliance:**
    *   Strict adherence to "Citation Standards" (e.g., limited quote length, no more than one quote per source, original summaries).
    *   Strict adherence to "Copyright Compliance" (e.g., no reproduction of song lyrics, poems, extensive quotes, or copyrighted content in code blocks/artifacts).
6.  **Output:** Synthesized information, with proper attribution and summaries in the agent's own words, is returned, typically to the Orchestrator or the requesting mode.

### 4. Problem Escalation and Resolution Workflow

This workflow handles issues that a specialist mode cannot resolve on its own.

1.  **Issue Identification:** A specialist mode encounters an issue outside its direct scope or capability (e.g., Code mode identifies a design flaw, a generalist mode encounters a complex runtime error).
2.  **Escalation Trigger:** The mode identifies the need to escalate based on the "Mode Delegation Matrix" (e.g., schema changes → Architect; runtime/test issues → Debug; unclear user intent → Ask).
3.  **Escalation to Orchestrator:** The issue is reported back to the Orchestrator, likely following the boomerang pattern (original subtask may be paused).
4.  **Re-delegation for Resolution:** The Orchestrator creates a new subtask specifically for resolving the identified issue and delegates it to the appropriate specialist mode (e.g., Debug mode for technical issues, Architect mode for design issues).
5.  **Problem Diagnosis & Solution:** The assigned specialist mode applies its structured methodology (e.g., Debug mode uses diagnostic steps; Architect mode reviews and proposes design changes).
6.  **Validation:** The proposed solution is validated.
7.  **Reporting:** The resolution and outcome are reported back to the Orchestrator.
8.  **Resumption:** The Orchestrator integrates the solution, and the original paused task (if any) can resume, or the project proceeds with the fix in place.

### 5. Ethics Escalation Workflow

This workflow addresses potential ethical concerns.

1.  **Concern Identification:** Any mode, during its operation, identifies a potential ethical issue based on the "Ethics Layer" core principles (truthfulness, transparency, etc.) or escalation flags (e.g., `ethics_violation`, `coercion_risk`, `uncertain_truth`, `privacy_breach_possible`).
2.  **Escalation:** The concern is flagged and escalated. While the specific recipient is not explicitly detailed for direct escalation from a specialist mode, the Orchestrator is the central hub and would likely be involved.
3.  **Handling (Implicit):** The documented framework focuses on flagging. The subsequent handling process is not detailed but would typically involve review by the Orchestrator and potentially human oversight to determine the appropriate course of action.

These workflows illustrate a sophisticated, structured, and interconnected system for managing complex tasks using a multi-agent approach.
