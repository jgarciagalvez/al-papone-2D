### Project "Al Papone": Guiding Principles & Technology Stack

This document outlines the core technical decisions and development philosophy for the project. All subsequent design and implementation work should adhere to these guidelines to ensure a consistent, efficient, and maintainable workflow.

#### I. Guiding Principles

This project is primarily developed by a Large Language Model (LLM), a constraint that dictates our architectural priorities. The primary goal is to create a "glass box" environment that is transparent, modular, and easy for an AI to reason about by leveraging it for its core strengths.

1.  **LLM-Driven Development:** The development process is driven by an LLM. The architecture must prioritize clarity and debuggability for the AI. 
    
2.  **Fail Fast Error Strategy:** Any potential errors should be caught early and cause failure rather than silently failing. No optimistic handling. Errors should be completely interpretable by LLMs.
    
3.  **Extreme Modularity:** The codebase will be highly decoupled, favoring small, pure functions that operate on well-defined data. This allows the LLM to work with isolated, manageable contexts.
    
4.  **Explicit Data Structures:** All data schemas will be defined explicitly and simply. We will avoid complex, inherited class structures in favor of plain data objects to ensure the AI has a clear understanding of the application's state.
    
5.  **Maximum Observability:** Tracing and logging should be able to be turned on at low levels, and any errors or logging should give complete information for an LLM to interpret and pin point the exact issue or situation.
    
6.  **High-Velocity Development:** Tooling and workflows are chosen to support rapid iteration and a tight feedback loop (code -> build -> test).
    
7.  **Modern & Focused Target:** The initial target is desktop-only, supporting modern browsers. Legacy browser support and mobile compatibility are not current priorities.
    

#### II. Technology Stack

The technology stack is selected to directly support the principles above, prioritizing control and transparency over monolithic frameworks.

*   **Language:** **TypeScript**
    
    *   _Reasoning:_ Provides static typing for creating explicit data structures (#4) and is a mainstream language well-understood by LLMs.
        
*   **Rendering Engine:** **Kontra.js**
    
    *   _Reasoning:_ This minimalist micro-library provides a "glass box" at the game logic level. It handles the optimized, low-level, and repetitive tasks (game loop, sprite management, input handling) without introducing a complex, opaque framework. This prevents the LLM from having to reinvent a game engine, directly supporting High-Velocity Development (#6). Its tiny API surface keeps the debugging focus on our own code and state (#2), making it ideal for an LLM-driven workflow (#1).
        
*   **Build Tool & Development Environment:** **Vite**
    
    *   _Reasoning:_ Delivers a high-velocity development cycle (#6) through its instant dev server and Hot Module Replacement (HMR). It provides out-of-the-box support for TypeScript, streamlining the entire workflow.