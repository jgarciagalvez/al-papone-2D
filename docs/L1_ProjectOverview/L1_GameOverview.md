# Game Overview: Al Papone (MVP)

## 1. Core Concept

* **Game Title:** Al Papone
* **Genre:** 2D Run-and-Gun Platformer
* **Player Fantasy:** You are a tough, agile potato gangster in a 1920 vegetable world. A father whose organization has been shattered, on a lone mission of vengeance and rescue.
* **Logline:** After his former protégé stages a devastating coup and kidnaps his son, lone potato mobster Al Papone must wage a one-man war against the ruthless "Root Syndicate" to save his family.

## 2. Narrative & World

### Characters
* **Al Papone (Player):** The respected "Spudfather" of the Tuber underworld. After a devastating betrayal shattered his organization, his sole focus is rescuing his son.
* **Spud Jr.:** Al Papone's son, kidnapped by the Root Syndicate.
* **Rose Papone:** Al Papone's wife, kidnapped by the Root Syndicate.
* **The Root Syndicate (Enemies):** A ruthless gang of "taproot" vegetables. Their ranks are filled with Carrot grunts, Radish brawlers, and Parsnip snipers, led by Al's former second-in-command.
    * **Silas 'Slim' Parsnip:** The main antagonist. A cunning and cruel strategist who betrayed Al Papone to forge his own empire.
* **The Potato Police (Enemies):** Corrupted cops who are on Silas's payroll and help protect him.

### MVP Story Arc
The story follows a clear, three-act rescue mission.
* **Level 1: The Betrayal:** Silas and his new Root Syndicate attack Al Papone's main speakeasy. In the chaos, they kidnap his family, forcing Al to begin his relentless pursuit.
* **Level 2: City Under Siege:** Al Papone fights his way across the city, now swarming with Root Syndicate thugs and corrupt police, tracking his family's captors.
* **Level 3: The Root Cellar:** Al Papone locates and infiltrates the Syndicate's main hideout for the final, desperate confrontation to save his family.

## 3. Core Gameplay & Systems

### Core Loop
The gameplay is built on a simple, engaging loop: **Run -> Shoot -> Platform -> Survive**. The player is constantly pushed forward to engage enemies and navigate the environment.

### Player Controls
* **Move:** Left / Right
* **Jump:** Can shoot while jumping.
* **Shoot:** 4-directional aiming (Up, Down, Left, Right).
* **Use Special:** Throws a grenade.

### Lives & Scoring
* **Lives:** Player starts with 3 lives. Losing a life resets the player to a recent checkpoint and reverts their weapon to the default.
* **Scoring:** A simple arcade-style system. Players earn points for defeating enemies, with a bonus awarded at the end of the level based on completion time.

## 4. Weapon System

### Default Weapon
* **Standard Tommy Gun:** The player's default weapon. Fires standard potato starch pellets and has infinite ammo.

### Topping Power-ups
These are temporary weapon upgrades. When a player picks up a topping, their weapon is instantly upgraded for a limited ammo count. When the ammo is depleted or the player loses a life, the weapon automatically reverts to the Standard Tommy Gun.

| Topping Power-up | Weapon Name | Firing Mechanic |
| :--- | :--- | :--- |
| **Bacon Bits** | The Scattergun | Short-range, 5-pellet spread shot. Ideal for crowd control. |
| **Melted Cheese**| The Globber | Fires a single, slow-moving projectile that deals low damage but slows enemies on impact. |
| **Jalapeños** | The Spicy Cannon| Fires a single projectile that explodes for high area-of-effect (AoE) damage. |

### Special Weapon
* **"Cherry Bomb" Grenades:** A limited-use explosive. Players can find more in the levels and can hold a maximum of 3 at a time.

## 5. Level Design & Structure

### Progression Style
Levels are continuous, side-scrolling environments where the camera follows the player's horizontal (left-to-right) progress.

### Platforming Elements
Levels will feature verticality and obstacles integrated thematically into the city environment:
* Fire escapes
* Construction scaffolding
* Building rooftops
* Stacked crates
* Moving trams

### Level Breakdown
* **Level 1: The Betrayal**
    * **Setting:** The fiery, chaotic remains of Al Papone's main speakeasy and the adjoining back alleys.
    * **Climax:** A showdown with a **Syndicate Captain** (a tough, shielded Carrot) who is coordinating the attack.

* **Level 2: City Under Siege**
    * **Setting:** City streets and rooftops, filled with fighting against the Syndicate and the Police.
    * **Climax:** A tense platforming challenge across stationary train cars, culminating in a battle against two elite **Syndicate Captains**.

* **Level 3: The Root Cellar**
    * **Setting:** The industrial warehouse district, leading into the Root Syndicate's fortified hideout.
    * **Climax:** The final confrontation for Act 1. The player must defeat Silas's monstrous bodyguard, **"The Crusher"** (a giant Turnip), to rescue his family, setting the stage for a future showdown with Silas himself.

## 6. MVP Scope

### IN SCOPE for MVP
* 3 complete levels with tiered bosses.
* 2 basic enemy types: **Root Syndicate Grunt (Carrot)** and **Potato Cop**.
* 1 mini-boss (Syndicate Captain), 1 vehicle boss (Paddy Wagon), 1 final boss (The Crusher).
* The 3-topping weapon power-up system plus the standard gun.
* "Cherry Bomb" grenade system.
* Basic life and scoring systems.

### OUT OF SCOPE for MVP
* Additional enemy types like Parsnip Snipers or Radish Brawlers (reserved for future development).
* Complex collectibles or secret-finding systems.
* Experience points, skill trees, or character progression.
* Story cutscenes beyond simple still images.
* 8-directional aiming.

## 7. Storytelling Method

The narrative will be delivered through simple, cost-effective methods suitable for solo development:
* **Comic-Style Still Images:** The intro and transitions between levels will be presented as a series of still images with text boxes, establishing the plot and character motivations without complex animation.