### Project "Al Papone": Guiding Principles & Technology Stack

This document outlines the core technical decisions and development philosophy for the project. All subsequent design and implementation work should adhere to these guidelines to ensure a consistent, efficient, and maintainable workflow.

#### I. Guiding Principles

This project is developed entirely by a Large Language Model (LLM), a constraint that dictates our architectural priorities. The primary goal is to create a "glass box" environment that is transparent, modular, and easy for an AI to reason about.

1.  **LLM-Centric Architecture:** The entire development process is handled by an LLM. The architecture must prioritize clarity and debuggability for the AI over concessions made for human developer convenience (e.g., boilerplate reduction).
2.  **Transparent Debugging:** The system must be fully auditable. The LLM needs to trace operations and errors through our own codebase, not into opaque, third-party libraries. The entire call stack must be visible.
3.  **Extreme Modularity:** The codebase will be highly decoupled, favoring small, pure functions that operate on well-defined data. This allows the LLM to work with isolated, manageable contexts. An **Entity Component System (ECS)** approach is favored.
4.  **Explicit Data Structures:** All data schemas will be defined explicitly and simply. We will avoid complex, inherited class structures in favor of plain data objects to ensure the AI has a clear understanding of the application's state.
5.  **Maximum Observability:** The code must feature extensive logging and explicit error handling at all stages. This provides the necessary feedback for the LLM to identify and resolve issues.
6.  **High-Velocity Development:** Tooling and workflows are chosen to support rapid iteration and a tight feedback loop (code -> build -> test).
7.  **Modern & Focused Target:** The initial target is desktop-only, supporting modern browsers. Legacy browser support and mobile compatibility are not current priorities.
8.  **Mainstream Language:** The chosen language (TypeScript) must be well-supported and extensively represented in the LLM's training data.

#### II. Technology Stack

The technology stack is selected to directly support the principles above, prioritizing control and transparency over pre-built frameworks.

*   **Language:** **TypeScript**
    *   *Reasoning:* Provides static typing for creating explicit data structures (#4) and is a mainstream language well-understood by LLMs (#8).

*   **Rendering Engine:** **Raw HTML5 Canvas API**
    *   *Reasoning:* Guarantees a "glass box" architecture. We build the entire rendering loop, ensuring complete control over the call stack, which is critical for transparent debugging (#2) and maximum observability (#5). This approach allows for a truly modular and explicit system design (#3, #4).

*   **Build Tool & Development Environment:** **Vite**
    *   *Reasoning:* Delivers a high-velocity development cycle (#6) through its instant dev server and Hot Module Replacement (HMR). It provides out-of-the-box support for TypeScript, streamlining the entire workflow.