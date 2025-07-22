# L2 Module: GameManager

The `GameManager` is the application's central orchestrator. It manages the high-level game state and controls the lifecycle of primary `System` modules.

The authoritative definition of all game states and their valid transitions is located in `docs/L1_ProjectOverview/L1_HighLevelFlow.md`. This document specifies the `GameManager`'s responsibilities within each of those states.

## Initial Setup

Before any state becomes active, the `GameManager` performs a one-time setup:
1.  Integrates its own `update` function with the `Kontra.js` game loop.
2.  Initializes its internal state machine to the `InitialBoot` state.

## Game State Logic

This section details the `GameManager`'s actions for each game state. The teardown of a state's active `System` is always handled by the `OnEnter` logic of the subsequent state to centralize all transition logic.

### `InitialBoot`

**Description:** Manages the initial game launch, loading global assets before the main menu appears.
**Active System:** `BootSystem`
**Active Services:**
*   `AssetService`
*   `AudioService`

**OnEnter Logic:**
1.  Instantiates the `BootSystem` and provides it with the designated `Active Services`.
2.  Sets `BootSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `BootSystem`.

**Possible Transitions:**
*   `-> MainMenu`

### `MainMenu`

**Description:** Manages the interactive main menu, allowing the player to start a game or view other menus.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `PersistenceService`
*   `AssetService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`BootSystem`, `MenuSystem` from another menu, or a frozen `GameplaySystem`).
2.  Instantiates the `MenuSystem` and provides it with the designated `Active Services`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> LevelLoad`
*   `-> ViewLeaderboard`

### `ViewLeaderboard`

**Description:** Manages the display of high scores.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `PersistenceService`
*   `AssetService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`MainMenu`).
2.  Instantiates the `MenuSystem` and provides it with the designated `Active Services`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> MainMenu`

### `LevelLoad`

**Description:** Manages the transition between levels, showing a cutscene while loading assets in the background.
**Active System:** `StorySystem`
**Active Services:**
*   `AssetService`
*   `LevelService`
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`MenuSystem` or `StorySystem`).
2.  Instantiates the `StorySystem` and provides it with the designated `Active Services`.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> InGame`

### `InGame`

**Description:** Manages the core gameplay loop.
**Active System:** `GameplaySystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `LevelService`
*   `HUDService`
*   `AssetService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`StorySystem`).
2.  If resuming from a `Paused` state, makes the existing `GameplaySystem` active again.
3.  Otherwise, instantiates a new `GameplaySystem` and provides it with the designated `Active Services`.
4.  Sets `GameplaySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `GameplaySystem`.

**Possible Transitions:**
*   `-> Paused`
*   `-> LevelComplete`
*   `-> PlayerDead`

### `Paused`

**Description:** Freezes the `GameplaySystem` and displays a pause menu.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `AssetService`

**OnEnter Logic:**
1.  Makes the `GameplaySystem` inactive, preserving its state in memory.
2.  Instantiates the `MenuSystem` and provides it with the designated `Active Services`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> InGame`
*   `-> MainMenu`

### `LevelComplete`

**Description:** Manages the sequence shown after a player successfully completes a level.
**Active System:** `StorySystem`
**Active Services:**
*   `AssetService`
*   `LevelService`
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`GameplaySystem`).
2.  Instantiates the `StorySystem` and provides it with the designated `Active Services`.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> LevelLoad`
*   `-> PlayerWin`

### `PlayerWin`

**Description:** Manages the celebration sequence shown after the player completes the final level.
**Active System:** `StorySystem`
**Active Services:**
*   `AssetService`
*   `LevelService`
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`StorySystem`).
2.  Instantiates the `StorySystem` and provides it with the designated `Active Services`.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> ScoreEntry`

### `PlayerDead`

**Description:** Manages the final death sequence after all player lives are used.
**Active System:** `StorySystem`
**Active Services:**
*   `AssetService`
*   `LevelService`
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`GameplaySystem`).
2.  Instantiates the `StorySystem` and provides it with the designated `Active Services`.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> ScoreEntry`

### `ScoreEntry`

**Description:** Manages the UI for the player to enter their name for the leaderboard.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `PersistenceService`
*   `AssetService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`StorySystem`).
2.  Instantiates the `MenuSystem` and provides it with the designated `Active Services`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> MainMenu` 