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
        *   **`LoadingSystem`**
            *   **Role:** Manages loading screens and presents narrative cutscenes. It orchestrates the `AssetService` and `LevelService` to prepare for the next state.
            *   **Active States:** `InitialLoad`, `LoadingLevel`.
            *   **Uses Services:** `AssetService`, `LevelService`, `AudioService`.
        *   **`MenuSystem`**
            *   **Role:** Handles all interactive, non-gameplay UI (main menu, pause, leaderboards).
            *   **Active States:** `MainMenu`, `ViewLeaderboard`, `LevelComplete`, `Paused`, `GameOver`, `GameComplete`.
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

*   **`InitialLoad`**: `LoadingSystem` is active. It loads global assets.
    *   **→** `MainMenu`
*   **`MainMenu`**: `MenuSystem` is active. Player can start game or view scores.
    *   **→** `LoadingLevel` or `ViewLeaderboard`
*   **`ViewLeaderboard`**: `MenuSystem` is active. Shows high scores.
    *   **→** `MainMenu`
*   **`LoadingLevel`**: `LoadingSystem` is active. It loads level assets and displays cutscenes while `LevelService` prepares the level in the background.
    *   **→** `InGame`
*   **`InGame`**: `GameplaySystem` is active. Core gameplay is active.
    *   **→** `Paused`, `LevelComplete`, or `GameOver`
*   **`Paused`**: The GameplaySystem is made inactive, freezing the game and preserving its state. The MenuSystem becomes active to display the pause menu.
    *   **→** `InGame` (the `GameplaySystem` is made active again) or `MainMenu`
*   **`LevelComplete`**: `MenuSystem` is active. Shows score summary.
    *   **→** `LoadingLevel` (next level) or `GameComplete`
*   **`GameOver`**: `MenuSystem` is active. Player has lost all lives.
    *   **→** `MainMenu`
*   **`GameComplete`**: `MenuSystem` is active. Player has finished the final level.
    *   **→** `MainMenu`

### 3. Module Summary Table

<table>
  <thead>
    <tr>
      <th>Module</th>
      <th>Type</th>
      <th>Description</th>
      <th>Services Used</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="4" style="text-align: center;"><strong>Orchestrator</strong></td>
    </tr>
    <tr>
      <td><code>GameManager</code></td>
      <td>Orchestrator</td>
      <td>Manages state transitions, activates Systems.</td>
      <td>-</td>
    </tr>
    <tr>
      <td colspan="4" style="text-align: center;"><strong>Systems</strong></td>
    </tr>
    <tr>
      <td><code>LoadingSystem</code></td>
      <td>System</td>
      <td>Manages loading screens and cutscenes.</td>
      <td><code>AssetService</code>, <code>LevelService</code>, <code>AudioService</code></td>
    </tr>
    <tr>
      <td><code>MenuSystem</code></td>
      <td>System</td>
      <td>Handles all interactive non-gameplay menus.</td>
      <td><code>InputService</code>, <code>AudioService</code>, <code>PersistenceService</code>, <code>AssetService</code></td>
    </tr>
    <tr>
      <td><code>GameplaySystem</code></td>
      <td>System</td>
      <td>Runs core game loop, physics, and logic.</td>
      <td><code>InputService</code>, <code>AudioService</code>, <code>LevelService</code>, <code>HUDService</code>, <code>AssetService</code></td>
    </tr>
    <tr>
      <td colspan="4" style="text-align: center;"><strong>Services</strong></td>
    </tr>
    <tr>
      <td><code>AssetService</code></td>
      <td>Service</td>
      <td>Loads, unloads, and provides access to raw assets.</td>
      <td>-</td>
    </tr>
    <tr>
      <td><code>InputService</code></td>
      <td>Service</td>
      <td>Abstracts physical inputs to logical actions.</td>
      <td>-</td>
    </tr>
    <tr>
      <td><code>AudioService</code></td>
      <td>Service</td>
      <td>Plays sound/music data provided by a System.</td>
      <td>-</td>
    </tr>
    <tr>
      <td><code>LevelService</code></td>
      <td>Service</td>
      <td>Parses level data provided by a System.</td>
      <td>-</td>
    </tr>
    <tr>
      <td><code>HUDService</code></td>
      <td>Service</td>
      <td>Renders HUD data provided by a System.</td>
      <td>-</td>
    </tr>
    <tr>
      <td><code>PersistenceService</code></td>
      <td>Service</td>
      <td>Saves/loads persistent data (e.g., scores).</td>
      <td>-</td>
    </tr>
  </tbody>
</table>
