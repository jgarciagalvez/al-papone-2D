# L2 Schema: GameSession

The `GameSession` schema defines the data structure for tracking a player's state throughout a single, continuous playthrough, from starting a new game to reaching a "game over" or "game win" a condition.

This object is instantiated by the `GameManager` when a new game session begins. The `GameManager` then passes **scoped references** to the sub-objects (`PlayerProfile`, `SessionProgress`, `Scoring`) to the `Systems` that require them, ensuring each `System` only has access to the data it needs.

## Schema Definition

```typescript
interface GameSession {
  playerProfile: PlayerProfile;
  sessionProgress: SessionProgress;
  scoring: Scoring;
}

interface PlayerProfile {
  /**
   * The number of lives the player has remaining.
   * When this reaches 0, the game session ends.
   * Typically starts at a default value (e.g., 3).
   */
  lives: number;

  /**
   * The number of "Cherry Bomb" grenades the player is currently holding.
   * Can be capped at a maximum value (e.g., 3).
   */
  grenadeCount: number;
}

interface SessionProgress {
  /**
   * The identifier for the current level being played or loaded.
   * This is used by the LevelService to fetch the correct level data.
   * Example: "level_01"
   */
  currentLevelId: string;

  /**
   * Tracks the highest level the player has successfully completed.
   * Used to determine which level to load next.
   * Example: 0 for none, 1 after completing level 1.
   */
  highestLevelCompleted: number;
}

interface Scoring {
  /**
   * The player's total score, which accumulates across all levels within the session.
   * Resets to 0 at the start of a new session.
   */
  totalScore: number;

  /**
   * Stores the time taken to complete the current level, in seconds.
   * This can be used to calculate a time-based bonus at the end of the level.
   * Resets at the start of each level.
   */
  currentLevelTime: number;
}
``` 