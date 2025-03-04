# Introduction
This article presents basic information about common resources that you see in Tekken Movesets. Bandai Namco has been using the same animation engine that they've been using since Tekken 1 but with changes built on top of it in each game. I suggest having this [spreadsheet](https://docs.google.com/spreadsheets/d/1DBkC-HfqD0KWQNeOTKjJWmPxdbEuCcGZxkPxQpsLkOY/edit?usp=sharing) open as you explore this article.

Huge thanks to `Sadamitsu` who explained some of the things I wasn't clear about and `Dennis` for proof-reading the documentation.

More resources will be documented in the future.

### Please Note
The structures described here are based on the latest game, *Tekken 8*. While there may be minor differences in structures across different games in the series, please note that these explanations are specifically tailored for *Tekken 8*. You can still get a decent idea for how Movesets work for prior games through this documentation.

# Moves
Anything that involves an animation is categorized as a "Move." This includes intros, win poses, attacks, taunts, throws, reactions to throws, Rage Arts, Heat Smashes, real-time story mode cutscenes, and stage transition animations. Each and every one of these is considered a "Move."


### Consists of
- Move Name/Key
- Animation Name/Key
- Hit level (High, mid, low, unblockable)
- Hurtbox / Vulnerability
- List of [Cancels](#cancel)
- List of [Hit Conditions](#hit-conditions)
- List of Hit/Block sounds (optional)
- List of [Extra Move Properties](#extra-move-properties) (optional)
- List of [Move Start Properties](#move-startend-properties) (optional)
- List of [Move End Properties](#move-startend-properties) (optional)
- List of Hitbox Values (optional, non-attack moves don't have hitbox values)


### A Move's corelation to other resources
```
Move
  ├─ Cancels
  │   ├─ Cancel 1
  │   │   ├─ Group Cancels
  │   │   │   ├─ Group Cancel 1
  │   │   │   ├─ Group Cancel 2
  │   │   │   └─ Group Cancel 3
  │   │   ├─ List of Requirements
  │   │   └─ Cancel Extradata
  │   ├─ Cancel 2
  │   │   ├─ Input Sequence
  │   │   │   ├─ Input 1
  │   │   │   ├─ Input 2
  │   │   │   └─ Input 3
  │   │   ├─ List of Requirements
  │   │   └─ Cancel Extradata
  │   └─ Cancel 3
  │       ├─ List of Requirements
  │       └─ Cancel Extradata
  ├─ Hit Conditions
  │   ├─ Hit Condition 1
  │   │   ├─ List of Requirements
  │   │   └─ List of Reactions
  │   │       └─ Pushback
  │   │           └─ Pushback Extradata
  │   └─ Hit Condition 2
  ├─ Voice Clips
  │   ├─ Voiceclip 1
  │   │   ├─ Folder ID
  │   │   └─ Voice ID
  │   └─ Voiceclip 2
  │       ├─ Folder ID
  │       └─ Voice ID
  ├─ Extra Move Properties
  │   ├─ Property 1
  │   │   ├─ List of Requirements
  │   │   ├─ Property ID
  │   │   └─ Frame: 0x4XXX (constant execution)
  │   ├─ Property 2
  │   │   ├─ List of Requirements
  │   │   ├─ Property ID
  │   │   └─ Frame: 0x8XXXX (instant execution)
  │   ├─ Property 3
  │   │   ├─ List of Requirements
  │   │   └─ Property ID (Can invoke a Throw Camera resource)
  │   │       └─ Throw
  │   │           └─ Throw Extradata
  │   ├─ Property 4
  │   │   ├─ List of Requirements
  │   │   └─ Property ID (Can invoke a Projectile resouce)
  │   │       └─ Projectile
  │   └─ Property 5
  │       ├─ List of Requirements
  │       └─ Property ID (Can invoke a Dialogue resource)
  │           └─ Dialogue
  ├─ Move Start Properties
  │   ├─ Start Property 1
  │   │   ├─ List of Requirements
  │   │   └─ Property ID
  │   └─ Start Property 2
  │       ├─ List of Requirements
  │       └─ Property ID
  ├─ Move End Properties
  │   ├─ End Property 1
  │   │   ├─ List of Requirements
  │   │   └─ Property ID
  │   └─ End Property 2
  │       ├─ List of Requirements
  │       └─ Property ID
  └─ Move Hitbox
      ├─ Hitbox 1
      ├─ Hitbox 2
      ├─ Hitbox 3
      ├─ Hitbox 4
      ├─ Hitbox 5
      ├─ Hitbox 6
      ├─ Hitbox 7
      └─ Hitbox 8

```

# Cancel
If you imagine moves as nodes in a graph, then cancels are the edges connecting them. Cancel resources define how and why a move transitions into another move.
A cancel occurs only when the necessary commands and conditions are met. Each move has at least one cancel, and the end of a cancel list is marked by a cancel with a command value of `0x8000`.
<br/>
### Consists of
- Command
- List of [Requirements](#requirement)
- [Cancel Extra Data](#cancel-extra-data) (Additional cancel properties)
- Input Detection Window (Start)
- Input Detection Window (End)
- Cancel Frame
- Move ID
- Cancel Type

### List of cancels Found in
- [Moves](#moves)
- Projectiles

### Structure
```cpp
struct tk_cancel
{
  uint64_t command;
  tk_requirement *requirements;
  tk_cancel_extradata *extradata;
  uint32_t input_window_start;
  uint32_t input_window_end;
  uint32_t starting_frame;
  uint16_t move_id;
  uint16_t type;
};
```
This structure has been the same since Tekken 6 and onwards

### Types of cancels
- Cancel into a move by Command and meeting [Requirements](#requirement) (if any)
- Cancel into a move by meeting [Requirements](#requirement) (if any)
- No canceling, invoke [Extra Properties](#extra-move-properties) upon meeting Command and [Requirements](#requirement) (if any)
- No canceling, invoke [Extra Properties](#extra-move-properties) upon meeting [Requirements](#requirement) (if any)

It's the `type` property that determines what sort of cancel it is.


### Cancel - Command
A Cancel's command describes the input required to invoke it. The command value can be broken down into 2 things:
- Directional Input
- Button Input<br/>

As you may have guessed it, directional inputs are movement inputs, such as `forward`, `back`, `neutral`, `down+forward` etc...
<br/>Meanwhile Button inputs represent attack buttons. E.g, `RP`, `LK`, `LP + RP`, `Rage Art` and even `Heat`.
<br/>
<br/>A cancel can actually refer to other resources such as an `Input Sequence` or a list of `Group Cancels`.
- If a cancel has a command value of or greater than `0x8014`, then that means it is referring to an `Input Sequence` resource. It's fairly straight forward, `0x8014` is the first sequence, `0x8015` would be the second, `0x8016` would be the third and so on.
- If a cancel has a command value of `0x8012`, that means, it is telling the game to look inside `Group Cancels`. In this case, `move_id` value dictates the starting point in the group cancels array. Value `N` would mean to start looking from Nth `Group Cancel` and keep going until a command value of `0x8013` is reached, `0x8013` dictates the end of a group cancel list.
- If a cancel has a command of `0x8000`, that means it is the end of the cancel list for that move and it is the last cancel.
- Some command values are pre-fixed. E.g, `0x8001` means to double-tap forward. You can find more in the [spreadsheet](https://docs.google.com/spreadsheets/d/1DBkC-HfqD0KWQNeOTKjJWmPxdbEuCcGZxkPxQpsLkOY/edit?usp=sharing)

### Cancel - List of Requirements
List of conditions that need to be fulfilled for the cancel to happen. This could also be a list of proprties that need to be executed based on some conditions. Refer to the [Requirement](#requirement) resource.

### Cancel - Cancel Extra Data
Additional properties to apply on the cancel. Description of the resouce is [below](#cancel-extra-data)
- Sometimes, you may want to cancel into another move while retaining your current frame count. Let me explain with an example: Kazuya's Spinning Demon (or "Hellsweep") [CD+4]. On its own, CD+4 never knocks down the opponent. However, if you follow it with CD+4,1 or CD+4,4, you'll notice the first hit causes the opponent to trip. Why is this? Behind the scenes, there are two visually identical moves for CD+4, differing only in their hit reactions. Let’s call the standalone CD+4 `Move A`, and the initial part of CD+4,1 or CD+4,4 `Move B`. When you input CD+4 on its own, `Move A` is performed. But if you press 1 or 4 during the move, it transitions seamlessly into `Move B` without any visual difference. What makes this transition smooth is a mechanism called `Cancel Extra Data`. If the value is set to `1024`, it ensures the frames of the current move carry over during the transition. For example, if you press 1 during the 10th frame of CD+4, the game transitions from `Move A` to `Move B` while keeping the frame count at 10. This means `Move B` starts from the same frame instead of resetting to frame 1. This frame continuity is what enables the smooth transition between the two moves.
- For airborne cancels, like Akuma's air-flips from Tekken 7 or Clive's jump moves from Tekken 8, the game, by default puts the character on the ground when a new move starts. But it's prevented by having a `Cancel Extra Data` value of `128`.
- The way throws work, both the action & the reaction animations are played on the character & the opponent simultaneously while the distance b/w them is decreased to 0. That happens when Cancel Extradata is `6876 (0x1ADC)`. It picks the reaction animation from the [Hit Condition](#hit-conditions) and applies it to the opponent.

### Cancel - Input Detection Window
These start & end values determine how long you have to make your input. And the values are the frame number of the current move that is currently being performed. If a move has input detection start-value as 10 and end-value as 27, that means you have `27 - 10 = 17` frames to do your input. If the detection end value is `32767`, it means you have the entire duration of the current move to do an input.

### Cancel - Cancel Frame
At what frame of the current move, it should cancel to the next move. If this and the Detection Window values are 0, that means the cancel frame is the end of the animation. For attack moves, it's pretty rare for the last frame of the animation to be the point where it cancels back to the idle stance. E.g, EWGF is 50 frames long but it cancels back to idle at frame 36.

### Cancel - Move ID
The move it should cancel into.
- If the value is `>= 32768 (0x8000)` then it means we're cancelling to generic moves. E.g, `32769` is the default standing stance, `32770` is the default crouching stance. You should check the `Generic IDs` tab in the spreadsheet for more information on this.
- If the `Cancel Command` is `0x8012`, which signifies a group cancel, then Move ID holds the starting point in group cancel list. E.g, Move ID # 100 would mean to start looking from group cancel # 100 and keep looking until we reach the `End of Group Cancel list value` which is `0x8013`.
- If a move ID is zero, that means to keep cancelling into the same move over & over again.

### Cancel - Cancel Type
As a general guideline, if the value is even, it represents a manual cancel, while an odd value indicates an automatic cancel. A manual cancel requires specific commands to be input in addition to meeting the necessary conditions. In contrast, an automatic cancel only requires the conditions to be fulfilled, with no additional input needed.

# Requirement

Numerical values represent specific conditions that must be satisfied for the parent entity to take effect. These conditions, known as requirements, are frequently used in various contexts. Some requirements depend on specific parameter values, while others do not. The requirements are evaluated sequentially, and if any condition fails, the evaluation stops immediately.

Requirements start from `0` and go up to around `1100` (for Tekken 8), whereas property values begin at `0x8001`. This distinction between Requirements and [Extra Move Properties](#extra-move-properties) is crucial.

For example (all values are according to Tekken 8):
- Value `0` means "Always True"
- Value `667` checks if the game is in story mode.
- Value `44` verifies if the attack that this cancel is attached to, was successful or not.
- Value `350` determines whether the player is in Devil mode while playing as Kazuya, based on the provided parameter (`0` for no, `1` for yes).
- Value `505` checks if the player character ID matches any of the specified parameter values.
- Value `1100` signifies the end of a requirement list

Refer to the [spreadsheet](https://docs.google.com/spreadsheets/d/1DBkC-HfqD0KWQNeOTKjJWmPxdbEuCcGZxkPxQpsLkOY/edit?usp=sharing) for detailed descriptions of all requirement values.


### Consists of
- Condition/Requirement Value
- Parameter 1 (if any)
- Parameter 2 (if any)
- Parameter 3 (if any)
- Parameter 4 (if any)

### Structure
<details>
  <summary>Tekken 8</summary>

```cpp
// Parameters can be signed, unsigned or float
union tk_param
{
  uint32_t param_unsigned;
  int32_t param_signed;
  float param_float;
};

struct tk_requirement
{
  uint32_t req;
  tk_param params[4];
};

```
</details>


<details>
  <summary>Tekken 6/Tag 2/7</summary>

```cpp
// Parameters can be signed, unsigned or float
union tk_param
{
  uint32_t param_unsigned;
  int32_t param_signed;
  float param_float;
};

struct tk_requirement
{
  uint32_t req;
  tk_param param;
};

```
</details>


### Found in
- [Cancels](#cancel)
- [Hit Conditions](#hit-conditions)
- Cutscene Dialogues
- [Extra Move Properties](#extra-move-properties)
- [Move Start/End Properties](#move-startend-properties)

### How it works
To demonstrate, I have this example requirement list from an attack's cancel
```
44              - If successful hit
66              - If opponent is standing upright
0x87F2, 9       - Play sound from opponent (grunt)
0x87F2, 1, 25   - Play sound from opponent (hit VFX)
0x8002          - Camera Shake - Light variant
1100, 0         - End of the list
```

Firstmost value is the requirement, while the values proceeded by semi-colons are parameters. Here the game checks if the player lands a hit on an upright opponent so it can play some Audio & Video effects.<br/>
- The way requirement lists work is that they are linked lists that end when they encounter an End-of-List value, which is `1100` for the current game version.
- If any requirement turns out to be `false`, the game stops checking the entire list.
- If a requirement value is `>= 0x8001` then it's not a Requirement, it's an [Extra Move Property](#extra-move-properties).
- If instead of a requirement value, the list stores an [Extra Move Property](#extra-move-properties), it will simply be executed/applied on the current move. In the above example, the list stores 2 Properties "Play Sound" and "Camera Shake", those will be applied to the current move as soon as the hit lands on an upright opponent.

Above described structure would look like this in memory:
```
Req #   Offset   req       param1     param2     param3     param4     // Description
--------------------------------------------------------------------------------------
1       0x0000   0x002C    0x0000     0x0000     0x0000     0x0000     // If successful hit
2       0x0014   0x0042    0x0000     0x0000     0x0000     0x0000     // If opponent is standing upright
3       0x0028   0x87F2    0x0009     0x0000     0x0000     0x0000     // Play sound from opponent (grunt)
4       0x003C   0x87F2    0x0001     0x0019     0x0000     0x0000     // Play sound from opponent (hit VFX)
5       0x0050   0x8002    0x0000     0x0000     0x0000     0x0000     // Camera Shake - Light variant
6       0x0064   0x044C    0x0000     0x0000     0x0000     0x0000     // End of the list

```

# Extra Move Properties
As the name implies, these are optional properties that can be added to a move. While a Move resource includes its Hitbox and Hurtbox values, other elements—such as modifying flags, controlling hand or facial gestures, consuming tornado effects, or triggering audio and visual effects—are managed through these lists.

Property values starts from `0x8001`. That's a crucial difference between requirements and properties. Requirements start from `0` and go till around `1100` (for Tekken 8)

### Consist of
- Frame Number
- List of Requirements
- Property ID
- 5 Parameter values (if any)

### Structure
<details>
  <summary>Tekken 8</summary>

```cpp
// Parameters can be signed, unsigned or float
union tk_param
{
  uint32_t param_unsigned;
  int32_t param_signed;
  float param_float;
};

struct tk_extraprops
{
  uint32_t frame;
  uint32_t _0x4; // unused
  uint32_t property;
  tk_requirement *requirements;
  tk_param params[5];
};
```
</details>

<details>
  <summary>Tekken 6/Tag 2/7</summary>

```cpp
// Parameters can be signed, unsigned or float
union tk_param
{
  uint32_t param_unsigned;
  int32_t param_signed;
  float param_float;
};

struct tk_extraprops
{
  uint32_t frame;
  uint32_t property;
  tk_param param;
};
```
</details>

### How it works
This is the list of properties attached to Kazuya's idle stance
```
Frame      Property   Parameter 1
0x8001,    0x81EF
80,        0x801E
0x8001,    0x8611,    118
0x8001,    0x8610,    119
0,         0
```
Let's break this down one property at a time:
- We don't really know what property `0x81EF` does, but it's frame value of `0x8001 (32769)` means that it should be applied to the current move exactly one frame after this move starts playing. How did we get know it should be the first frame? Sometimes, a move may cancel into another move but not on Frame 1 (let's say frame # 5). When that happens, if an Extra Property has a frame value 1, then it will be skipped since first frame will not be played. In that case, we set the frame to `0x8001 (32769)`. If we want to activate the property at 2nd frame **after** it's been cancelled into, we set the frame to `0x8002 (32770)` and so on.
- I believe `0x801E` property is used for playing mouth fog VFX on snowy/winter stages. Here, the frame value is 80, meaning it'll be applied on 80th frame of Kazuya's idle stance
- Property `0x8611` and `0x8610` are for applying hand animations on right & left hands respectively. These values have parameters that are the animation indexes. And both of these properties are being applied instantly as soon as we cancel back into the idle stance (Frame: `0x8001`)
- Lastly, we frame & property value both as 0, which indicate the end of the property list.
- If starting frame value is `0x4XXX` then the property is executed every frame starting from Frame `XX`. E.g, `0x400A` would mean to start executing the property on every frame from 10th (0xA) frame and onwards

Above mentioned Extra Properties list would look like this in memory (hexadecimal)
```
Prop #  Offset   Frame    _0x4     Property  Req Ptr        Param1   Param2   Param3   Param4   Param5  // Description
------------------------------------------------------------------------------------------------------------------------
1       0x0000   0x8001   0x0000   0x81EF    <PTR>          0x0000   0x0000   0x0000   0x0000   0x0000  // Unknown property
2       0x0020   0x0050   0x0000   0x801E    <PTR>          0x0000   0x0000   0x0000   0x0000   0x0000  // Mouth Fog VFX
3       0x0040   0x8001   0x0000   0x8611    <PTR>          0x0076   0x0000   0x0000   0x0000   0x0000  // Right Hand animation
4       0x0060   0x8001   0x0000   0x8610    <PTR>          0x0077   0x0000   0x0000   0x0000   0x0000  // Left Hand animation
5       0x0080   0x0000   0x0000   0x0000    <PTR>          0x0000   0x0000   0x0000   0x0000   0x0000  // End of list
```

# Move Start/End Properties
This resource is similar to [Extra Move Properties](#extra-move-properties) but with one key difference: it does not include a "Starting Frame" attribute. As the name suggests, a move can have a list of properties assigned to it, which are executed either just before the move begins or after it ends. The list continues until it encounters the "End of List" requirement, marking its conclusion.

### Consists of
- List of Requirements
- Property ID
- 5 Parameter values (if any)

### Structure
<details>
  <summary>Tekken 8</summary>

```cpp
// Parameters can be signed, unsigned or float
union tk_param
{
  uint32_t param_unsigned;
  int32_t param_signed;
  float param_float;
};

struct tk_start_end_props
{
  tk_requirement *requirements;
  uint32_t property;
  tk_param params[5];
};
```
</details>

<details>
  <summary>Tekken 6/Tag 2/7</summary>

```cpp
// Parameters can be signed, unsigned or float
union tk_param
{
  uint32_t param_unsigned;
  int32_t param_signed;
  float param_float;
};

struct tk_extraprops
{
  tk_requirement *requirements;
  uint32_t property;
  tk_param param;
};
```
</details>
<br/>
They look like this in memory in hexadecimal view:

```
<PTR to Requirements> | 000084CC | 00000010 00000020 00000000 00000000 00000000
<PTR to Requirements> | 000082A5 | 0000000A 00000000 00000000 00000000 00000000
<PTR to Requirements> | 0000044C | 00000000 00000000 00000000 00000000 00000000
```

Property `1100` (0x44C) denotes the end of the list

# Hit Conditions
Resource that is used to dictate which animations to apply on the opponent when an attack move connects, this deals with both the hit & block scenarios, while also navigating standing as well as airborne opponents.

Note: When we say "standing" in context of reactions and hit conditions, please know that it means opponent that have their feet on the ground, which includes both standing and crouching states.

### Consists of
- List of [Requirements](#requirement)
- Damage
- [Reaction List](#reaction-list)

### Found in
- [Moves](#moves)
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
  <summary>Tekken 6/Tag 2/7/8</summary>

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

# Reaction List
This resource determines the reaction animations played on the opponent when an attack move connects. It defines the specific animation triggered when the attack is blocked, lands as a normal hit, strikes from different angles (front, side, or behind), or registers as a counter hit. The term "Reaction List" refers to a single instance of this resource, named as such because it contains a list of possible reaction state values.

- Each [Hit Condition](#hit-conditions) item has a corresponding `Reaction List` item
- Reaction animations are all part of the same moveset that triggers an action. For example, reaction animations to King's throws are not stored in every character's moveset individually. Instead, they are stored within King's moveset and are selected and played from there.

### Consists of
- [List of Pushback for different angles/states](#list-of-pushback-for-different-anglesstates)
- [List of "Direction of Pushback" for different angles/states](#list-of-direction-of-pushback-for-different-anglesstates)
- [List of "Rotation of Pushback" for different angles/states](#list-of-rotation-of-pushback-for-different-anglesstates)
- [List of reaction move IDs for different angles/states](#list-of-reaction-move-ids-for-different-anglesstates)

### List of Pushback for different angles/states
These refer to `Pushback` resource. More on them later.
  - Front
  - Back
  - Left
  - Right
  - Front (Counter Hit)
  - Downed
  - Block

### List of "Direction of Pushback" for Different Angles/States  

The path or the trajectory in which the opponent will be pushed. When an attack connects and a reaction animation is applied, the opponent's movement can be influenced diagonally. These attributes control that behavior. For example, Kazuya’s **df2 on Counter Hit** slightly pushes the opponent to his right, causing them to become slightly off-axis, this effect is achieved because the trajectory on CH is slightly diagonal.

This mini-structure consists of the following values:

- Front
- Back
- Left
- Right
- Front (Counter Hit) / Vertical Pushback for Airborne Opponents
- Downed

To understand and calculate the precise rotation value, [refer here](#rotation-value-calculation)

**Note 1:** The field responsible for *Front* pushback deals with both Hit & Block scenarios<br/>
**Note 2:** The field responsible for *Front (Counter Hit)* also determines the additional height applied to an airborne opponent when the move connects.

### List of "Rotation of Pushback" for different angles/states
These attributes determine how an opponent turns when an attack connects.

For example, when Kazuya's **df2 lands on Counter Hit**, the opponent's facing direction shifts slightly to the left of the player upon impact.  

- If an opponent completely turns around after certain attacks, it's due to these rotation values. For instance, a front-hit rotation value may be set to force a **180-degree turn**, causing the opponent to fully rotate upon being hit.

This rotation sub-structure consists of:
- Front
- Back
- Left
- Right
- Front (Counter Hit)
- Downed

To understand and calculate the precise rotation value, [refer here](#rotation-value-calculation)

### List of reaction move IDs for different angles/states

This sub-structure decides what actual move to play for the corresponding situation. They store the move indexes from the source character's moveset. It contains the following values/fields:
  - Standing / Default
  - Crouching
  - Standing (Counter Hit)
  - Crouching (Counter Hit)
  - Left Side
  - Left Side (Crouching)
  - Right Side
  - Right Side (Crouching)
  - Back Side
  - Back Side (Crouching)
  - Block
  - Block (Crouching)
  - Wall-splatted
  - Downed

Here is how an example reaction list item would look like in memory (hexadecimal)
```
Offset   Example Value                  // Description
-----------------------------------------------------------
0x0000   <PTR to pushback item>         // Front
0x0008   <PTR to pushback item>         // Back
0x0010   <PTR to pushback item>         // Left
0x0018   <PTR to pushback item>         // Right
0x0020   <PTR to pushback item>         // Front (Counter Hit)
0x0028   <PTR to pushback item>         // Downed
0x0030   <PTR to pushback item>         // Block

0x0038   0x0000                         // Front Direction
0x003A   0x0000                         // Back Direction
0x003C   0x0000                         // Left Side Direction
0x003E   0x0000                         // Right Side Direction
0x0040   0x0000                         // Front Counterhit Direction
0x0042   0x0000                         // Downed Direction

0x0044   0x0000                         // Front Rotation
0x0046   0x0000                         // Back Rotation
0x0048   0x0000                         // Left Side Rotation
0x004A   0x0000                         // Right Side Rotation
0x004C   0x0000                         // Vertical Pushback
0x004E   0x0000                         // Downed Rotation

0x0050   0x0182 (Move ID 386)           // Standing
0x0052   0x0182                         // Crouch
0x0054   0x0182                         // CH
0x0056   0x0182                         // Crouch CH
0x0058   0x0182                         // Left Side
0x005A   0x0182                         // Left Side Crouch
0x005C   0x0182                         // Right Side
0x005E   0x0182                         // Right Side Crouch

0x0060   0x0182                         // Back
0x0062   0x0182                         // Back Crouch
0x0064   0x0625 (Move ID 1573)          // Block
0x0066   0x0829 (Move ID 2089)          // Crouch Block
0x0068   0x0107 (Move ID 263)           // Wallslump
0x006A   0x0109 (Move ID 265)           // Downed
0x006C   0x0000                         // Unused
0x006E   0x0000                         // Unused

```

### Structure
<details>
  <summary>Tekken 6/Tag 2/7/8</summary>

```cpp
struct tk_reaction
{
  // Pushbacks
  tk_pushback *front_pushback;
  tk_pushback *backturned_pushback;
  tk_pushback *left_side_pushback;
  tk_pushback *right_side_pushback;
  tk_pushback *front_counterhit_pushback;
  tk_pushback *downed_pushback;
  tk_pushback *block_pushback;

  // Directions in which the pushback will be applied
  uint16_t front_direction; // hit & block
  uint16_t back_direction;
  uint16_t left_side_direction;
  uint16_t right_side_direction;
  uint16_t front_counterhit_direction;
  uint16_t downed_direction;

  // Rotations of the body when a reaction is applied
  uint16_t front_rotation;
  uint16_t back_rotation;
  uint16_t left_side_rotation;
  uint16_t right_side_rotation;
  uint16_t front_counterhit_rotation; // also 'vertical_pushback'
  uint16_t downed_rotation;

  // Move IDs
  uint16_t standing;
  uint16_t crouch;
  uint16_t ch;
  uint16_t crouch_ch;
  uint16_t left_side;
  uint16_t left_side_crouch;
  uint16_t right_side;
  uint16_t right_side_crouch;
  uint16_t back;
  uint16_t back_crouch;
  uint16_t block;
  uint16_t crouch_block;
  uint16_t wallslump;
  uint16_t downed;
  uint16_t unk1; // unused
  uint16_t unk2; // unused
};
```
</details>

# Cancel Extra Data
These are 4-byte bit-flags that dictate additional properties for cancels. You should refer to the tab of the same name in the [spreadsheet](https://docs.google.com/spreadsheets/d/1DBkC-HfqD0KWQNeOTKjJWmPxdbEuCcGZxkPxQpsLkOY/edit?usp=sharing). Each moveset has around 50-60 of these values. These flags can do many things at once. Some of the additional properties include
- Retaining the height for airborne moves like Clive's jumping attacks
- Retaining the frame number when transitioning from Move A to Move B
- Shaving off starting frames of the next move
- Playing the next move animation backwards
- Track to one or both side as the cancel happens
- Play both action & reaction animations on the characters (used in throws)

Some example scenarios are mentioned [here](#cancel---cancel-extra-data)


## Rotation Value Calculation

To obtain the 2-byte hexadecimal value that Tekken uses for rotation in degrees, follow this straightforward calculation:
```
if      0 degrees = 0x0000
and   360 degrees = 0xFFFF
then     1 degree = 0xFFFF / 360 = 0xB6 (or 182 in decimal)

        X degrees = 0xB6 * X
```
E.g, `45 degrees` would be `0x1FFF` or `8191`

I'll just quote the exact words that `Sadamitsu` said to me when I asked him about it,

```
I think it would be easier to understand if you imagine a clock face with hexadecimal numbers. Like in the movie The Martian (2015). In this case:
0x0000 - is like 12 o'clock.
0x4000 - is like 3 o'clock.
0x8000 - is like 6 o'clock.
0xC000 - is like 9 o'clock.
```