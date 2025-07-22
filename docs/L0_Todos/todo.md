### Homework Summary: Preparing for L2 by Detailing the Game Design

**Objective:** The goal of this homework is to move from the high-level L1 architecture to the detailed L2 design. To do this effectively, we need to define the specific mechanics and attributes of the game. Your descriptions will serve as the direct input for creating the L2 `schemas` (data models) for characters, items, and levels.

**Deliverable:** Create a new document named `L2 - Game Design`.

**Key Areas to Detail in the Document:**

1.  **Characters (Player & Enemies):**
    * For the characters active in the first three levels, define their specific attributes. Think in terms of parameters that differentiate them.
    * **Examples:** What is their movement speed? How high can they jump? What are their health points? Do they have unique abilities?

2.  **Levels:**
    * Describe what makes each of the first three levels unique.
    * **Examples:** Are there different layouts or platforms? Does the number or type of enemies increase? Are there new environmental hazards? Does the difficulty ramp up in a specific way?

3.  **Weapons & Power-ups:**
    * Go into detail on the mechanics of each weapon and power-up.
    * **For weapons (e.g., Tommy Gun):** What is its rate of fire? What is the projectile's speed and range? Does it have infinite ammo as planned?
    * **For power-ups (e.g., Cherry Bomb):** How is it thrown (arc, speed)? What is the explosion's radius and damage? How long does it take to explode?

4.  **Bosses & Environment:**
    * What makes a "boss" character different from a standard enemy? What are its specific mechanics?
    * How can the player interact with the environment? Can they climb on certain platforms? Are there destructible elements?

5.  **Audio:**
    * Begin listing the sound effects the game will need (e.g., jumping, shooting, enemy hits, explosions, background music for each level).

**Guiding Principle:** As you define these details, try to think from an "abstracted level." Look for common parameters you can use to describe different game elements (e.g., all characters have a `speed` attribute, but the value is different for each). This will make it much easier to translate your design into a structured data model.