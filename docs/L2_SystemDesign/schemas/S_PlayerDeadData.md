# L2 Schema: PlayerDeadData

The `PlayerDeadData` schema defines the structure of the ephemeral payload object used when transitioning from `InGame` to `PlayerDead`.

This object is created by the `GameplaySystem` when the player runs out of lives. It provides the `StorySystem` with context about the cause of the final death, allowing for a more specific and meaningful "game over" sequence.

## Schema Definition

```typescript
interface PlayerDeadData {
  /**
   * Describes how the player was ultimately defeated.
   * - 'ENEMY_CONTACT': Player was defeated by touching an enemy.
   * - 'PROJECTILE': Player was defeated by an enemy projectile.
   * - 'HAZARD': Player was defeated by an environmental hazard (e.g., spikes).
   */
  causeOfDeath: 'ENEMY_CONTACT' | 'PROJECTILE' | 'HAZARD';
}
``` 