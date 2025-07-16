### L1 High-Level Flow: The "Central Conductor" Model

This document outlines the high-level application flow and the core modules that structure the game.

#### 1. High-Level Flow

The game's architecture is built on a "Central Conductor" pattern. This pattern uses a single `GameManager` module to control the main application state and orchestrate all other independent systems. This design ensures a flat hierarchy and a single source of truth for the game's overall state, aligning with our principles of modularity and clarity.

##### 1.1. Application Flow & State Definitions

The `GameManager` orchestrates the game by moving through the following defined states. This provides a single, clear source of truth for the application's flow. The memory management strategy (swapping level assets) is a key part of this design.

*   **`InitialLoad`**
    *   **Purpose:** Loads all **Global Assets** (e.g., player sprite, UI elements, fonts, menu backgrounds) into persistent memory for the duration of the session.
    *   **Transitions:** Automatically enters this state on application start. On completion, transitions to `MainMenu`.

*   **`MainMenu`**
    *   **Purpose:** Acts as the main hub. Presents the player with options to "Start Game" or "View Leaderboard".
    *   **Transitions:** Entered from `InitialLoad`, `GameComplete`, or `GameOver`. On "Start Game," transitions to `LoadingLevel`.

*   **`LoadingLevel`**
    *   **Purpose:** The core transition state for managing memory. It **unloads** the assets from the previous level (if one exists) and **loads** the assets for the upcoming level. During this process, it displays the relevant narrative cutscenes (e.g., combining the end cutscene of Level 1 with the intro cutscene for Level 2).
    *   **Transitions:** Entered from `MainMenu` or `InGame`. On completion, transitions to `InGame`.

*   **`InGame`**
    *   **Purpose:** The active gameplay state where the player has control. Upon level completion, the game logic determines if it was the final level.
    *   **Transitions:**
        *   Entered from `LoadingLevel` or `Paused`.
        *   Transitions to `Paused` if the player pauses.
        *   Transitions to `GameOver` if the player runs out of lives.
        *   Transitions to `LoadingLevel` if a level is completed and it is **not** the final level.
        *   Transitions to `GameComplete` if the **final** level is completed.

*   **`Paused`**
    *   **Purpose:** Freezes the gameplay and shows a pause menu with options to resume or quit.
    *   **Transitions:** Entered from `InGame`. On resume, transitions back to `InGame`.

*   **`GameComplete`**
    *   **Purpose:** The "You Win!" state. Displays the final cutscene and game credits, then allows the player to view the leaderboard and save their score.
    *   **Transitions:** Entered from `InGame`. Upon player action, transitions to `MainMenu`.

*   **`GameOver`**
    *   **Purpose:** The "You Lose" state. Displays the player's final score alongside the leaderboard and allows the player to enter their name to save their high score.
    *   **Transitions:** Entered from `InGame`. Upon player action, transitions to `MainMenu`.

#### 2. Core Modules & Communication

The architecture consists of several independent modules orchestrated by the `GameManager`.

##### 2.1. Module Communication Contract

Modules are "passive" and do not communicate directly with each other. They signal their status back to the `GameManager` via a consistent contract: each module's `update()` function must return a status object. This ensures all communication is explicit and reliable.

##### 2.2. Module Responsibilities

*   **`GameManager`**: The Conductor. Manages the main game state (`MainMenu`, `InGame`, etc.) and the primary game loop. Orchestrates all other modules based on the returned status objects.
*   **`AssetManager`**: The Quartermaster. Manages a two-stage loading process: an initial load for global assets (e.g., player sprite, UI elements) and on-demand loading for level-specific assets requested by the `LevelManager`.
*   **`MenuSystem`**: The Lobby. Manages the logic, rendering, and user input for all non-gameplay screens, such as the Main Menu, Options, and Leaderboard.
*   **`LevelManager`**: The Stagehand. Requests raw level data from the `AssetManager` and assembles it into a level-specific "script" object containing all logic and data for that level (e.g., enemy spawn patterns, event triggers).
*   **`GameplaySystem`**: The Engine. A generic system that executes the core gameplay loop. It consumes the "script" object provided by the `LevelManager` to run the specific rules of the current level, keeping it decoupled from any level-specific design.
*   **`UISystem`**: The HUD. Renders the in-game Heads-Up Display, showing real-time information like score, lives, and ammunition.
