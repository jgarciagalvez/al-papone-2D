### L1 High-Level Flow: The State-Driven Module Model

This document outlines the game's high-level architecture. It is built on a "State-Driven Module" model where a central always active `GameManager` orchestrates the application's flow by activating and deactivating different Systems based on the current game state.

### 1. Module Architecture & Hierarchy

The architecture is composed of three distinct module types: 
*  A single **Orchestrator** (`GameManager`)
*  State-specific **Systems** 
*  Independent **Services**

The GameManager makes a primary System active by running its update loop, or inactive by not running it. The GameplaySystem is unique: it can be made inactive to "freeze" it, preserving its full state in memory until it is made active again. Other Systems are typically created when needed and discarded after use.

*   **`GameManager` (Orchestrator)**
    *   **Role:** Manages game state transitions and activates/deactivates the primary Systems. It is a pure orchestrator and does not call any Services directly.
    *   **Uses Services:** None.
    *   **Controls Systems:**
        *   **`BootSystem`**
            *   **Role:** Manages the initial game launch, displaying a splash screen and loading global assets required for the main menu.
            *   **Active States:** `InitialBoot`.
            *   **Uses Services:** `AssetService`, `AudioService`.
        *   **`StorySystem`**
            *   **Role:** Manages and displays non-interactive story elements (cutscenes, death sequences, etc.). It orchestrates background loading of assets for the next active System.
            *   **Active States:** `LevelLoad`, `LevelComplete`, `PlayerWin`, `PlayerDead`.
            *   **Uses Services:** `AssetService`, `LevelService`, `AudioService`, `InputService`.
        *   **`MenuSystem`**
            *   **Role:** Handles all interactive, non-gameplay UI (main menu, pause, leaderboards).
            *   **Active States:** `MainMenu`, `ViewLeaderboard`, `Paused`, `ScoreEntry`.
            *   **Uses Services:** `InputService`, `AudioService`, `PersistenceService`, `AssetService`.
        *   **`GameplaySystem`**
            *   **Role:** Runs the core game loop, physics, and entity logic.
            *   **Active States:** `InGame`.
            *   **Uses Services:** `InputService`, `AudioService`, `LevelService`, `HUDService`, `AssetService`.

*   **Services (Independent Specialists)**
    *   Services are simple, independent modules that never talk to other services. They perform a single, well-defined task. The calling System is responsible for providing them with any required data or assets.
    *   **`AssetService`:** Loads, caches, and provides access to raw game assets.
    *   **`AudioService`:** Plays sounds and music provided to it.
    *   **`HUDService`:** Renders the in-game HUD based on data provided to it.
    *   **`InputService`:** Translates raw hardware input into logical game actions.
    *   **`LevelService`:** Parses raw level data and builds a scene structure from it.
    *   **`PersistenceService`:** Saves and loads persistent data (e.g., high scores).

### 2. High-Level State Flow

The `GameManager` transitions between states, each activating a primary System.

*   **`InitialBoot`**: `BootSystem` is active. It loads global assets.
    *   **→** `MainMenu`
*   **`MainMenu`**: `MenuSystem` is active. Player can start game or view scores.
    *   **→** `LevelLoad` or `ViewLeaderboard`
*   **`ViewLeaderboard`**: `MenuSystem` is active. Shows high scores.
    *   **→** `MainMenu` or `LevelLoad`
*   **`LevelLoad`**: `StorySystem` is active. It displays a cutscene while `AssetService` and `LevelService` prepare the next level in the background.
    *   **→** `InGame`
*   **`InGame`**: `GameplaySystem` is active. Core gameplay is active.
    *   **→** `Paused`, `LevelComplete`, or `PlayerDead` (on final life lost).
*   **`Paused`**: The `GameplaySystem` is made inactive, freezing the game and preserving its state. The `MenuSystem` becomes active to display the pause menu.
    *   **→** `InGame` (the `GameplaySystem` is made active again) or `MainMenu`
*   **`LevelComplete`**: `StorySystem` is active. Shows a level completion sequence.
    *   **→** `LevelLoad` (next level) or `PlayerWin` (game complete)
*   **`PlayerWin`**: `StorySystem` is active. Player has finished the final level and is shown a celebration scene.
    *   **→** `ScoreEntry`
*   **`PlayerDead`**: `StorySystem` is active. It plays a final death scene after all lives are used.
    *   **→** `ScoreEntry`
*   **`ScoreEntry`**: `MenuSystem` is active. Player enters their name for the leaderboard after winning or losing.
    *   **→** `MainMenu`
