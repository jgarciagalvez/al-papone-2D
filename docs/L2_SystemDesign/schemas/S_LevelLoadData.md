# L2 Schema: LevelLoadData

The `LevelLoadData` schema defines the structure of the ephemeral payload object used when transitioning to the `LevelLoad` state.

This object is created by the `System` initiating the transition (e.g., `MenuSystem` or `StorySystem`). It provides the `GameManager` with the necessary context to instruct the `StorySystem` on which level to prepare and what kind of transitional experience to present to the player.

## Schema Definition

```typescript
interface LevelLoadData {
  /**
   * The unique identifier for the level to be loaded. This ID is used
   * by the LevelService to fetch the correct level blueprint.
   * Example: "level_01"
   */
  levelId: string;

  /**
   * Provides context for why the level is being loaded. This allows the
   * StorySystem to display appropriate content, such as a full cutscene
   * for a new game versus a simple fade for a level restart.
   */
  loadContext: 'NEW_GAME' | 'NEXT_LEVEL' | 'RESTART_LEVEL' | 'CONTINUE_GAME';
}
```
