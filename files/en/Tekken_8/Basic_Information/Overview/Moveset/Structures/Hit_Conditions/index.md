# Hit Conditions
Resource that is used to dictate which animations to apply on the opponent when an attack move connects, this deals with both the hit & block scenarios, while also navigating standing as well as airborne opponents.

Note: When we say "standing" in context of reactions and hit conditions, please know that it means opponent that have their feet on the ground, which includes both standing and crouching states.

### Consists of
- List of [Requirements](../Requirement/)
- Damage
- [Reaction List](../Reaction_List/)

### Found in
- [Moves](../Move/)
- Projectiles

### How it works
Each move has a list of hit conditions attached to them. End of the list is dictated by encountering row that has a requirement value `1100` (which is the End-of-List value)

Below is an example Hit Condition List attached to a move
```
Damage, Reaction List, (Requirements)
20,     12,            (724: Tornado Available), (1100: End of list)
20,     6,             (723: Opponent is Airborne), (1100: End of list)
20,     181,           (1100: End of list)
```

- All Hit conditions have the same damage value, which is 20. The attack described above has 3 hit conditions and we can tell that it's an attack that inflicts Tornado on airborne opponents because of the present of the `Tornado Available` requirement.
- The list of requirements varies for each hit condition, determining which reaction will be triggered upon landing the attack. For example, if `Tornado Available` is met first, the reaction-list which have the Tornado effect will be played on the opponent instead of other reactions that might also apply.
- The order of hit conditions is important. If the `Opponent is Airborne` condition were placed before the `Tornado Available` condition, the Tornado effect would never trigger, as the airborne condition would always be met first, preventing the Tornado reaction from playing out.
- End of a Hit Condition list is determined by encountering a condition where the requirement list only has the `End of List` value.
- Attacks that don't have a dedicated airborne reaction tend to only have 1 Hit Condition (standing)
- Attacks that do have a dedicated airborne reaction tend to have 2 Hit Conditions (airborne, standing)
- Combo extender (Tornado/Screw) attacks tend to have 3 Hit Conditions (tornado, airborne, standing)
- Heat Engagers tend to have 6 Hit Conditions
  - Initial Heat Engagement
  - Heat Dash
  - Heat Dash in the same combo as Heat Burst
  - Heat Dash on Airborne opponents
  - Airborne opponents
  - Standing opponents

### Structure
<details>
  <summary>All Games (Post-Tekken 5)</summary>

```cpp
struct tk_hit_condition
{
  tk_requirement *requirements;
  uint32_t damage;
  uint32_t _0xC; // unused
  tk_reaction *reaction;
};
```
</details>
<br/>
