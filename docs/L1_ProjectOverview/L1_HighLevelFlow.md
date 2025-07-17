### L1 High-Level Flow: The State-Driven Module Model

This document outlines the game's high-level flow using a "State-Driven Module" architecture. A central `GameManager` is **always active**, controlling the application's overall state. Each state defines a specific, predictable configuration of active modules, ensuring a clear, modular, and easily understandable user journey from start to finish.

#### The User Journey: States and Active Modules

This section details the game's progression from the user's perspective, explicitly linking each step to its corresponding Game State and the modules that are active.

*   **1. Initial Load**
    *   **User Sees:** The application starts, displaying a loading screen as essential global assets are loaded into memory.
    *   **Game State:** `InitialLoad`
    *   **Active Modules:** `AssetManager` (loads global assets), `MenuSystem` (renders loading screen).

*   **2. Main Menu**
    *   **User Sees:** The main menu, with options to "Start Game" or "View Leaderboard".
    *   **Game State:** `MainMenu`
    *   **Active Modules:** `MenuSystem` (renders menu, handles input).

*   **3. View Leaderboard**
    *   **User Sees:** The high-score leaderboard. The user can return to the main menu.
    *   **Game State:** `ViewLeaderboard`
    *   **Active Modules:** `MenuSystem` (renders leaderboard, handles input).

*   **4. Loading Level & Cutscene**
    *   **User Sees:** When "Start Game" is selected or a level is completed, a transition screen is shown. This screen displays narrative cutscenes while loading the required level assets.
    *   **Game State:** `LoadingLevel`
    *   **Active Modules:** `AssetManager` (loads level assets), `LevelManager` (prepares level data), `MenuSystem` (renders cutscene/loading visuals).

*   **5. In-Game**
    *   **User Sees:** The core gameplay screen. The player controls Al Papone and interacts with the game world. The HUD displays lives, score, and ammo.
    *   **Game State:** `InGame`
    *   **Active Modules:** `GameplaySystem` (runs all game logic, physics, and controls), `UISystem` (renders the HUD).

*   **6. Level Complete**
    *   **User Sees:** After successfully completing a level, a summary screen appears showing score and time bonuses. The user can proceed to the next level.
    *   **Game State:** `LevelComplete`
    *   **Active Modules:** `MenuSystem` (renders level summary, handles input).

*   **7. Paused**
    *   **User Sees:** The game freezes and a pause menu appears with options to "Resume" or "Quit".
    *   **Game State:** `Paused`
    *   **Active Modules:** `MenuSystem` (renders pause menu, handles input). The `GameManager` signals the `GameplaySystem` and `UISystem` to become inactive, preserving their state until resumed.

*   **8. Game Over**
    *   **User Sees:** If all lives are lost, a "Game Over" screen appears, showing the final score and leaderboard. The user can enter their name and return to the main menu.
    *   **Game State:** `GameOver`
    *   **Active Modules:** `MenuSystem` (renders game over screen, handles input).

*   **9. Game Complete**
    *   **User Sees:** After completing the final level, a "You Win!" screen is displayed with credits. The user can save their score and return to the main menu.
    *   **Game State:** `GameComplete`
    *   **Active Modules:** `MenuSystem` (renders win screen/credits, handles input).

#### Module Activity Summary

The following table summarizes which modules are active in each game state. The `GameManager` is omitted as it is always active.

| Module           | Active States                                                                                      |
| :--------------- | :------------------------------------------------------------------------------------------------- |
| `AssetManager`   | `InitialLoad`, `LoadingLevel`                                                                      |
| `LevelManager`   | `LoadingLevel`                                                                                     |
| `GameplaySystem` | `InGame`                                                                                           |
| `UISystem`       | `InGame`                                                                                           |
| `MenuSystem`     | `InitialLoad`, `MainMenu`, `ViewLeaderboard`, `LoadingLevel`, `LevelComplete`, `Paused`, `GameOver`, `GameComplete` |
