# L2 Module: GameManager

The `GameManager` is the application's central orchestrator. It manages the high-level game state and controls the lifecycle of primary `System` modules using a hardcoded state machine. Its logic is imperative and explicit, built around a central `switch` statement to handle state transitions.

The authoritative definition of all game states and their valid transitions is located in `docs/L1_ProjectOverview/L1_HighLevelFlow.md`. This document specifies the `GameManager`'s responsibilities within each of those states.

## Data Management Strategy

The `GameManager` is the orchestrator for the application's **Centralized State Model**. It is the sole owner of all shared data objects (`GameSession`, `GameSettings`, `AssetLibrary`) and is responsible for managing the high-level game state machine.

When a state transition occurs, the `GameManager` instantiates the appropriate `System` for the new state and provides it with **scoped references** to only the specific data objects it needs to perform its function.

For a complete definition of the data flow and management strategy, see [L2_DataFlowAndManagement.md](../L2_DataFlowAndManagement.md).

## Initial Setup

Before any state becomes active, the `GameManager` performs a one-time setup:
1.  Integrates its own `update` function with the `Kontra.js` game loop.
2.  Initializes its internal state machine to the `InitialBoot` state.

## Game State Logic

The `GameManager`'s core is a central `transitionTo(state)` function containing a `switch` statement. When a transition is requested, this function executes the appropriate `case` block for the *new* state. The `OnEnter` logic detailed below for each state represents the body of that `case` block, which is responsible for tearing down the previous system and setting up the new one.

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

**Description:** Displays the game's leaderboard, showing player high scores. The player can start the game from this screen.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `PersistenceService`
*   `AssetService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`MenuSystem` from `MainMenu`).
2.  Instantiates the `MenuSystem` and provides it with the designated `Active Services`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> MainMenu`
*   `-> LevelLoad`

### `LevelLoad`

**Description:** Manages the transition to a new level, showing a story element or loading screen while asynchronously loading level-specific assets.
**Active System:** `StorySystem`
**Active Services:**
*   `AssetService`
*   `LevelService`
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`MenuSystem` from either `MainMenu` or `ViewLeaderboard`).
2.  Instantiates the `StorySystem`, providing it with the designated `Active Services` and a reference to `GameSession.sessionProgress`. The `StorySystem` reads the `currentLevelId` from this object to determine which level to load.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> InGame`

### `InGame`

**Description:** The main gameplay state where the player controls the character and interacts with the game world.
**Active System:** `GameplaySystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `LevelService`
*   `HUDService`
*   `AssetService`

**OnEnter Logic:**
1.  Handles teardown of the `StorySystem`.
2.  If this is a new game (not returning from `Paused`), instantiates the `GameplaySystem`, providing it with the designated `Active Services` and references to `GameSession.playerProfile`, `GameSession.sessionProgress`, and `GameSession.scoring`.
3.  If returning from `Paused`, it simply makes the existing `GameplaySystem` active again.
4.  Sets `GameplaySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `GameplaySystem`.

**Possible Transitions:**
*   `-> Paused`
*   `-> LevelComplete`
*   `-> PlayerDead`

### `Paused`

**Description:** Freezes the `InGame` state and displays the pause menu. The state of the `GameplaySystem` is preserved in memory.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`

**OnEnter Logic:**
1.  Makes the `GameplaySystem` inactive, preserving its state.
2.  Instantiates the `MenuSystem`, providing it with the designated `Active Services` and read-only access to `GameSession.playerProfile` and `GameSession.scoring`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> InGame`
*   `-> MainMenu`

### `LevelComplete`

**Description:** Displays a level completion sequence after the player finishes a level. It can transition to the next level or to the final win state.
**Active System:** `StorySystem`
**Active Services:**
*   `AssetService`
*   `LevelService`
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the `GameplaySystem`.
2.  Instantiates the `StorySystem`, providing it with the designated `Active Services`, the `LevelCompleteData` payload, and references to `GameSession.playerProfile`, `GameSession.sessionProgress`, and `GameSession.scoring`.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> LevelLoad` (for the next level)
*   `-> PlayerWin` (if it was the final level)

### `PlayerWin`

**Description:** Player has finished the final level and is shown a celebration scene.
**Active System:** `StorySystem`
**Active Services:**
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  The `StorySystem` is already active from the `LevelComplete` state; this state directs it to play the specific win sequence.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> ScoreEntry`

### `PlayerDead`

**Description:** Plays a final death scene after all lives are used.
**Active System:** `StorySystem`
**Active Services:**
*   `AudioService`
*   `InputService`

**OnEnter Logic:**
1.  Handles teardown of the `GameplaySystem`.
2.  Instantiates the `StorySystem`, providing it with the designated `Active Services` and references to `GameSession.scoring` and `GameSession.sessionProgress`.
3.  Sets `StorySystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `StorySystem`.

**Possible Transitions:**
*   `-> ScoreEntry`

### `ScoreEntry`

**Description:** Allows the player to enter their name for the leaderboard after winning or losing. From here, the player can restart the game or return to the main menu.
**Active System:** `MenuSystem`
**Active Services:**
*   `InputService`
*   `AudioService`
*   `PersistenceService`

**OnEnter Logic:**
1.  Handles teardown of the previous system (`StorySystem` from either `PlayerWin` or `PlayerDead`).
2.  Instantiates the `MenuSystem`, providing it with the designated `Active Services` and a reference to `GameSession.scoring`.
3.  Sets `MenuSystem` as the active system.

**OnUpdate Logic:**
1.  Delegates the `update` call to the active `MenuSystem`.

**Possible Transitions:**
*   `-> MainMenu`
*   `-> LevelLoad`

