# Reaction List
This resource determines the reaction animations played on the opponent when an attack move connects. It defines the specific animation triggered when the attack is blocked, lands as a normal hit, strikes from different angles (front, side, or behind), or registers as a counter hit. The term "Reaction List" refers to a single instance of this resource, named as such because it contains a list of possible reaction state values.

- Each [Hit Condition](../Hit_Conditions/) item has a corresponding `Reaction List` item
- Reaction animations are all part of the same moveset that triggers an action. For example, reaction animations to King's throws are not stored in every character's moveset individually. Instead, they are stored within King's moveset and are selected and played from there.

### Consists of
- [List of Pushback for different angles/states](#list-of-pushback-for-different-anglesstates)
- [List of "Direction of Pushback" for different angles/states](#list-of-direction-of-pushback-for-different-anglesstates)
- [List of "Rotation of Pushback" for different angles/states](#list-of-rotation-of-pushback-for-different-anglesstates)
- [List of reaction move IDs for different angles/states](#list-of-reaction-move-ids-for-different-anglesstates)

### List of Pushback for different angles/states
These refer to [Pushback](../Pushback/) resource. More on them later.
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

To understand and calculate the precise rotation value, [refer here.](#rotation-value-calculation)<br/>
[Here's a video example by Sadamitsu](https://drive.google.com/file/d/1X1y_CAgXG-IBF_J-w5kPFDHwqOVgj0KB/view)

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

To understand and calculate the precise rotation value, [refer here.](#rotation-value-calculation)<br/>
[Here's a video example by Sadamitsu](https://drive.google.com/file/d/1aP8cU1RuIWlSrwKX3J0Mxetg0U-one2G/view)

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
  <summary>All Games (Post-Tekken 5)</summary>

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
<br/>


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