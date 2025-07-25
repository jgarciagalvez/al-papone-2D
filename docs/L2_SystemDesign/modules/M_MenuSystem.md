# L2 Module: MenuSystem

The `MenuSystem` is responsible for handling all interactive, non-gameplay UI screens. It is activated by the `GameManager` in specific states to display menus, process user input, and initiate state transitions.

## Data & Service Dependencies

The `MenuSystem`'s interaction with other modules is well-defined. It consumes data from the `GameSession`, creates data payloads for transitions, and utilizes several core `Services`.

### Consumed `GameSession` Data

The `MenuSystem` receives **scoped, read-only references** to `GameSession` sub-objects depending on the active state:

*   **`Paused` State:** Receives `GameSession.playerProfile` and `GameSession.scoring` to display current lives and score.
*   **`ScoreEntry` State:** Receives `GameSession.scoring` to display the final score for the completed session.

### Generated Transition Payloads

To start gameplay, the `MenuSystem` creates and sends a `LevelLoadData` payload to the `GameManager`.

*   **`LevelLoadData`**: Created when the player opts to start a game from a menu. It specifies the `levelId` to load and provides a `loadContext` (e.g., `NEW_GAME`).
    *   *Schema*: `docs/L2_SystemDesign/schemas/S_LevelLoadData.md`

### Service Usage

The `MenuSystem` is provided with the following services by the `GameManager`:

*   **`InputService`**: To detect user interactions (button clicks, key presses).
*   **`AudioService`**: To play menu-related sound effects and music.
*   **`PersistenceService`**: To load high scores for the leaderboard and save new scores after a game session.
*   **`AssetService`**: To access UI assets like fonts, button sprites, and backgrounds.

## Behavior by State

The `MenuSystem`'s behavior is determined by the active game state.

### `MainMenu`

**Description:** Manages the main menu, the player's primary entry point into the game.

**UI & Logic:**
*   Displays options: "New Game" and "View Leaderboard".
*   Uses the `InputService` to listen for player selection.

**Possible Transitions:**
*   `-> LevelLoad`: When "New Game" is selected. The `MenuSystem` creates a `LevelLoadData` payload with `levelId` for the first level and `loadContext: 'NEW_GAME'`.
*   `-> ViewLeaderboard`: When "View Leaderboard" is selected.

### `ViewLeaderboard`

**Description:** Displays player high scores.

**UI & Logic:**
*   On activation, it requests high score data from the `PersistenceService`.
*   Renders the formatted leaderboard data on screen.
*   Provides navigation options: "New Game" and "Back to Main Menu".

**Possible Transitions:**
*   `-> MainMenu`: When the player chooses to go back.
*   `-> LevelLoad`: When "New Game" is selected. The `MenuSystem` creates a `LevelLoadData` payload as it does from the `MainMenu`.

### `Paused`

**Description:** Displays a pause menu when the `InGame` state is frozen.

**UI & Logic:**
*   Renders a pause overlay.
*   Displays the current player's `lives` and `totalScore` using the provided read-only `GameSession` references.
*   Provides options: "Resume" and "Quit to Main Menu".

**Possible Transitions:**
*   `-> InGame`: When "Resume" is selected, returning control to the `GameplaySystem`.
*   `-> MainMenu`: When the player chooses to abandon the current session.

### `ScoreEntry`

**Description:** Allows the player to enter their name for the leaderboard after a game session concludes.

**UI & Logic:**
*   Displays the player's `totalScore` from the provided `GameSession.scoring` reference.
*   Provides a text input field for the player to enter their name.
*   On submission, it sends the name and final score to the `PersistenceService` to be saved.
*   Provides navigation options: "Play Again" and "Return to Main Menu".

**Possible Transitions:**
*   `-> MainMenu`: When the player chooses to return to the main menu.
*   `-> LevelLoad`: When "Play Again" is selected. The `MenuSystem` creates a `LevelLoadData` payload to restart the game. 