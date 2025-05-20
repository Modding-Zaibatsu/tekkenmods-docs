# Reaction List

The **Reaction List** resource determines the opponent's animation when an attack connects. It defines the specific reaction based on:

* Hit type (normal, counter)
* Relative position (front, side, back)
* Blocked vs. landed attacks

Each reaction list item contains all relevant data for one reaction configuration.

---

## Key Notes

* Every [Hit Condition](../Hit_Conditions/) is paired with a `Reaction List` entry.
* Reaction animations originate from the **attacker’s** moveset, not the opponent's. For example, reaction animations to King’s throws are stored in King’s moveset and simply played on the opponent.

---

## Components

A Reaction List entry consists of:

* [Pushback values for different angles/states](#pushback-values)
* [Direction of pushback](#pushback-direction-values)
* [Rotation during pushback](#rotation-values)
* [Reaction move IDs](#reaction-move-ids)

---

## Pushback Values

Pushback refers to how the opponent is physically moved on hit. These are pointers to [Pushback](../Pushback/) resources, with different values based on hit angle and state:

* Front
* Back
* Left
* Right
* Front (Counter Hit)
* Downed
* Block

---

## Pushback Direction Values

These determine the **trajectory** of the pushback after a hit. Diagonal or off-axis movement is controlled here. For example, **Kazuya’s `df2` on counter hit** pushes the opponent slightly to the right, achieved via a customized direction value.

### Pushback Direction Fields:

* Front
* Back
* Left
* Right
* Front (Counter Hit)
* Downed

[See video example by Sadamitsu](https://drive.google.com/file/d/1X1y_CAgXG-IBF_J-w5kPFDHwqOVgj0KB/view)

---

## Rotation Values

These fields define how the opponent's orientation changes when a reaction is triggered.

Example:
If a front-facing attack causes the opponent to turn 180°, this is controlled via the *Front Rotation* value.

### Rotation Fields:

* Front
* Back
* Left
* Right
* Front (Counter Hit) — doubles as **vertical pushback** for airborne opponents
* Downed

[See rotation in action](https://drive.google.com/file/d/1aP8cU1RuIWlSrwKX3J0Mxetg0U-one2G/view)

---

## Reaction Move IDs

These fields determine **which animation/move** is played based on opponent state and angle.

### Fields:

* Standing (Front)
* Crouching
* Standing (Counter Hit)
* Crouching (Counter Hit)
* Left Side
* Left Side (Crouching)
* Right Side
* Right Side (Crouching)
* Back Side
* Back Side (Crouching)
* Block
* Block (Crouching)
* Wall Slump
* Downed

These are stored as **move indexes** within the attacker's moveset.

---

## Memory Layout Example (Hex View)

```text
Offset   Example Value      Description
-----------------------------------------------------------
0x0000   <PTR>              Front Pushback
0x0008   <PTR>              Back Pushback
0x0010   <PTR>              Left Pushback
0x0018   <PTR>              Right Pushback
0x0020   <PTR>              Front (Counter Hit) Pushback
0x0028   <PTR>              Downed Pushback
0x0030   <PTR>              Block Pushback

0x0038   0x0000             Front Direction
0x003A   0x0000             Back Direction
0x003C   0x0000             Left Direction
0x003E   0x0000             Right Direction
0x0040   0x0000             Front (CH)
0x0042   0x0000             Downed Direction

0x0044   0x0000             Front Rotation
0x0046   0x0000             Back Rotation
0x0048   0x0000             Left Rotation
0x004A   0x0000             Right Rotation
0x004C   0x0000             Front (CH) / Vertical Pushback
0x004E   0x0000             Downed Rotation

0x0050   0x0182             Standing Reaction (Move ID 386)
0x0052   0x0182             Crouch Reaction
0x0054   0x0182             CH Reaction
0x0056   0x0182             Crouch CH Reaction
0x0058   0x0182             Left Side
0x005A   0x0182             Left Side Crouch
0x005C   0x0182             Right Side
0x005E   0x0182             Right Side Crouch
0x0060   0x0182             Back
0x0062   0x0182             Back Crouch
0x0064   0x0625             Block
0x0066   0x0829             Crouch Block
0x0068   0x0107             Wall Slump
0x006A   0x0109             Downed
0x006C   0x0000             Unused
0x006E   0x0000             Unused
```

---

## Structure Definition

<details>
  <summary><strong>Applicable to Tekken 5 and later</strong></summary>

```cpp
struct tk_reaction
{
  // Pushbacks
  tk_pushback* front_pushback;
  tk_pushback* backturned_pushback;
  tk_pushback* left_side_pushback;
  tk_pushback* right_side_pushback;
  tk_pushback* front_counterhit_pushback;
  tk_pushback* downed_pushback;
  tk_pushback* block_pushback;

  // Direction values
  uint16_t front_direction;
  uint16_t back_direction;
  uint16_t left_side_direction;
  uint16_t right_side_direction;
  uint16_t front_counterhit_direction;
  uint16_t downed_direction;

  // Rotation values
  uint16_t front_rotation;
  uint16_t back_rotation;
  uint16_t left_side_rotation;
  uint16_t right_side_rotation;
  uint16_t front_counterhit_rotation; // also 'vertical_pushback'
  uint16_t downed_rotation;

  // Reaction move IDs
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

---

## Rotation Value Calculation

To compute Tekken's internal rotation value for a given degree:

```
360 degrees = 0xFFFF (65535 in decimal)
1 degree    = 0xFFFF / 360 ≈ 0xB6 (182 decimal)

Rotation Value = Degrees * 0xB6
```

**Examples:**

* `0 degrees` → `0x0000`
* `45 degrees` → `0x1FFF`
* `90 degrees` → `0x2D00`
* `180 degrees` → `0x5A00`
* `360 degrees` → `0xFFFF`

### Visual Aid (per Sadamitsu):

> Think of a clock face using hexadecimal:
>
> * `0x0000` = 12 o'clock
> * `0x4000` = 3 o'clock
> * `0x8000` = 6 o'clock
> * `0xC000` = 9 o'clock

This metaphor helps in visualizing rotation offsets.
