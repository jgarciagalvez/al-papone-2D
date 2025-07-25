# L2 Data Management: Centralized State Model

This document defines the application's data management strategy. The architecture uses a **Centralized State Model** where the `GameManager` acts as the sole owner of all shared game data. This model prioritizes clarity, decouples modules, and provides a predictable, unidirectional data flow.

## 1. Core Components

*   **`GameManager` (Orchestrator):**
    *   The single owner of all Global State Objects.
    *   Manages the game's state machine, activating and deactivating `Systems`.
    *   Provides active `Systems` with scoped references to the specific data objects they require.

*   **Global State Objects:**
    *   **`GameSession`:** Holds all transient data for a single playthrough (e.g., `lives`, `score`, `currentLevelId`). Reset for each new game.
    *   **`GameSettings`:** Holds all persistent user data (e.g., `audioVolume`, `keybindings`). Persists across game sessions.
    *   **`AssetLibrary`:** A central, in-memory registry of all loaded game assets and their current reference counts.

*   **`Systems` (Stateful Logic Modules):**
    *   Implement the logic for a specific game state (e.g., `GameplaySystem`, `MenuSystem`).
    *   They are stateful but do not own shared data; they operate on references provided by the `GameManager`.

*   **`Services` (Stateless Toolkits):**
    *   Collections of pure, static-like utility functions (e.g., `AssetService`, `PersistenceService`).
    *   They are stateless and operate exclusively on the data passed into their functions. They do not hold their own state.

## 2. Data Flow

The data flow is unidirectional and explicit, ensuring clear dependencies.

1.  **Initialization:** The `GameManager` instantiates the `System` for the current game state and injects its dependencies by passing scoped references to the required Global State Objects.
2.  **Operation:** The `System` executes its logic. To perform a specialized task, it invokes a function from a `Service`, passing the necessary data object as an argument.
    *   *Example:* `MenuSystem` calls `AudioService.playSound(soundId, gameSettings.audio)`.
3.  **State Modification:** The `Service` operates on the provided data object directly. Any modifications are immediately reflected in the global state.

## 3. Asset Management

Asset management uses a reference-counting model to automate memory management and prevent redundancy.

*   **Asset Manifests:** Each `System` or game state declares its dependencies in a declarative **Asset Manifest**—a simple list of required asset keys.
*   **Loading (`AssetService.load`):** When a `System` is activated, the `GameManager` uses its manifest to load assets. For each asset key:
    *   If the asset is not in the `AssetLibrary`, it is loaded, added to the library, and its reference count is set to `1`.
    *   If the asset already exists, its reference count is incremented.
*   **Unloading (`AssetService.release`):** When a `System` is deactivated, the `GameManager` uses its manifest to release assets. For each asset key:
    *   The asset's reference count is decremented.
    *   If the reference count reaches `0`, the asset is removed from the `AssetLibrary` and unloaded from memory.

This model guarantees that an asset remains in memory if and only if at least one active module requires it. 