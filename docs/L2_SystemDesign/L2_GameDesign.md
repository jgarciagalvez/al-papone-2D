# Character Roster & Characteristics

1 character object type, with different attributes depending on character type.
```json
{
  "id": "grunt_01",
  "type": "mele-grunt",
  "health": 30,
  "movementSpeed": 3,
  "attackDamage": 10,
  "behavior": "patrol",
  "pointsValue": 100
}
```

## Player Character
* **Al Papone (The Spudfather)**
    * **Role**: Player character.
    * **Movement**: Run (Left/Right), Jump.
    * **Offense**: 8-directional shooting. Access to Topping Power-ups. Can use "Cherry Bomb" grenades.
    * **Defense**: Starts with 3 lives.

## Standard Enemies (MVP)
* **Root Syndicate Grunt (Carrot)**
    * **Variant A: Melee Grunt**
        * **Role**: Basic close-quarters threat.
        * **Weapon**: Melee (e.g., Baton).
        * **Behavior**: Patrols a platform; attacks player on proximity.
    * **Variant B: Ranged Grunt**
        * **Role**: Low-level ranged pressure.
        * **Weapon**: Basic Handgun.
        * **Behavior**: Patrols, but stops to shoot when player is in line-of-sight.
* **Potato Cop**
    * **Role**: Stationary turret to control an area.
    * **Weapon**: Pistol.
    * **Behavior**: Stays in a fixed position, firing at the player on sight.

## Climax & Boss Characters (MVP)
* **Syndicate Captain**
    * **Role**: Mini-boss for Level 1 & 2 climax. A tough, shielded Carrot.
    * **Gimmick**: Carries a shield that blocks all frontal attacks.
    * **Vulnerability**: Must be attacked from the back or when it is shooting.
* **Paddy Wagon**
    * **Role**: Vehicle boss.
    * **Behavior**: Moves across the level, periodically spawning Root Syndicate Grunts.
    * **Vulnerability**: Destroy specific weak points like tires or the engine.
* **"The Crusher"**
    * **Role**: Final boss for the MVP, encountered in Level 3. A giant Turnip bodyguard.
    * **Behavior**: A slow, powerful, multi-phase fight.
    * **Attacks**: Includes a direct slam and a ground pound that causes environmental hazards (e.g., falling debris).

# Weapon Roster & Characteristics

Weapons should be managed as distinct objects that a character object references via a property like `equippedWeapon`. This approach allows for greater flexibility in swapping weapons and creating new ones.

## Player Weapons

### Standard Tommy Gun
* **Role**: Default, all-purpose weapon.
* **Ammo**: Infinite.
* **Damage**: Standard damage per shot.
* **Rate of Fire**: High.
* **Projectile Speed**: Fast.

### The Scattergun (Bacon Bits Power-up)
* **Role**: Short-range crowd control.
* **Ammo**: Limited count on pickup.
* **Mechanic**: Fires a 5-pellet spread shot.
* **Damage**: High if all pellets hit at close range.
* **Rate of Fire**: Low.
* **Range**: Short.

### The Globber (Melted Cheese Power-up)
* **Role**: Utility weapon for enemy control.
* **Ammo**: Limited count on pickup.
* **Mechanic**: Fires a projectile that slows enemies on impact.
* **Damage**: Standard, but takes a bit to kill. Enemies can kill if touched in this state.
* **Rate of Fire**: Medium.
* **Projectile Speed**: Medium.

### Spicy Rounds (Jalapeños Power-up)
* **Role**: Temporary damage boost for the Tommy Gun.
* **Ammo**: Limited count on pickup.
* **Mechanic**: Temporarily replaces the Tommy Gun's standard pellets with high-damage rounds. Firing mechanic (rate of fire, speed) remains the same as the Tommy Gun. Add distintive "thump" sound.
* **Damage**: High per-pellet damage.

### "Cherry Bomb" Grenade (Special Weapon)
* **Role**: High-impact explosive for clearing groups.
* **Ammo**: Limited use; can hold a maximum of 3.
* **Mechanic**: Thrown in an arc, explodes after a timer.
* **Damage**: High area-of-effect (AoE) damage.

---

## Enemy Weapons

### Grunt Melee Weapon (Baton/Club)
* **Role**: Basic close-quarters attack.
* **Mechanic**: Instant hit in a short arc.
* **Damage**: Low.
* **Rate of Fire**: Medium.
* **Range**: Melee only.

### Grunt Handgun
* **Role**: Basic ranged attack for Ranged Grunts.
* **Damage**: Low.
* **Rate of Fire**: Low.
* **Projectile Speed**: Slow.
* **Range**: Medium.

### Police Pistol
* **Role**: More effective ranged attack for Potato Cops.
* **Damage**: Medium.
* **Rate of Fire**: Medium, consistent.
* **Projectile Speed**: Medium.
* **Range**: Long.


## POST MVP Ideas
### Future Characters (Post-MVP)
* **Parsnip Sniper**
    * **Role**: Long-range, high-priority threat.
* **Radish Brawler**
    * **Role**: Tough, aggressive melee enemy that rushes the player.
* **Silas 'Slim' Parsnip**
    * **Role**: The main antagonist and presumed final boss of the full game.