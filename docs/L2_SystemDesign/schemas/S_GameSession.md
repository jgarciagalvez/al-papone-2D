# L2 Schema: GameSession

The `GameSession` schema defines the data structure for tracking a player's state throughout a single, continuous playthrough, from starting a new game to reaching a "game over" or "game win" condition.

This object is instantiated by the `GameManager` when a new game session begins (e.g., when the player clicks "New Game" in the main menu). It is then passed by reference to any `System` that needs to read or modify session-wide data, such as the `GameplaySystem` (to update score and lives) or the `HUDService` (to display the current state).

## Schema Definition

```typescript
interface S_GameSession {
  /**
   * The identifier for the current level being played or loaded.
   * This is used by the LevelService to fetch the correct level data.
   * Example: "level_01"
   */
  currentLevelId: string;

  /**
   * The player's score, which accumulates across all levels within the session.
   * Resets to 0 at the start of a new session.
   */
  playerScore: number;

  /**
   * The number of lives the player has remaining. 
   * When this reaches 0, the game session ends.
   * Typically starts at a default value (e.g., 3) and decrements when the player dies.
   */
  playerLives: number;
}
``` 