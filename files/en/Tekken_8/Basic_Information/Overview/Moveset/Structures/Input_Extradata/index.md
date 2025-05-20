# Input Extradata

An `Input Extradata` resource represents a single command input used within an [Input Sequence](../Input_Sequence/). It defines the list of inputs associated with a particular sequence.

---

## Structure Overview

Each input command is composed of two parts:

* **Directional Command**
* **Button Command**

---

## Directional Command

Directional commands specify movement directions. These values can be combined using bitwise addition to allow for multiple directional inputs.

**Directional Values:**

```
0x0000 - Any Direction
0x0002 - Down-Back (d/b)
0x0004 - Down (d)
0x0008 - Down-Forward (d/f)
0x0010 - Back (b)
0x0020 - Neutral (n)
0x0040 - Forward (f)
0x0080 - Up-Back (u/b)
0x0100 - Up (u)
0x0200 - Up-Forward (u/f)
```

**Example:**
To allow `d/b`, `d`, and `d/f`, combine their values:
`0x0002 + 0x0004 + 0x0008 = 0x000C`

---

## Button Command

Button commands define which attack buttons are involved, such as Left Punch (LP), Right Kick (RK), both punches (LP + RP), Rage Art (RA), etc.

The button command is a 4-byte hexadecimal value in the format:

```
0xMMNNHHPP
```

Where:

* `MM` - Input Mode
* `NN` - Buttons that must **not** be held
* `HH` - Buttons that **must** be held
* `PP` - Button(s) to be pressed

---

### MM (Input Mode)

Specifies the matching behavior of direction/button combinations:

```
0x00 - Unknown / Reserved
0x20 - Partial Input: Accept any of the specified buttons/directions
0x40 - Normal Input: Accept at least the specified button/direction
0x80 - Direction Only: Accept only the direction (no button input allowed)
```

**Examples:**

* `0x20`: Accepts input if **either** 1 or 2 is pressed (for a `1+2` command)
* `0x40`: Accepts `d/f+1`, but `d/f+1+2` or `d/f+1+3` will also be valid
* `0x80`: If a button is pressed with a direction, the input is invalid

---

### NN (Buttons That Must Not Be Held)

Indicates buttons that should **not** be held down:

```
0x00 - None
0x04 - Button 1 (LP) not held
0x08 - Button 2 (RP) not held
0x10 - Button 3 (LK) not held
0x20 - Button 4 (RK) not held
```

These values can be combined:
`0x04 + 0x08 = 0x0C` → Buttons 1 and 2 must not be held

---

### HH (Buttons That Must Be Held)

Indicates buttons that must be held during the input:

```
0x00 - None
0x02 - Button 1 (LP) held
0x04 - Button 2 (RP) held
0x08 - Button 3 (LK) held
0x10 - Button 4 (RK) held
```

Can also be combined:
`0x08 + 0x10 = 0x18` → Buttons 3 and 4 must be held

---

### PP (Buttons to Press)

Specifies the button(s) that should be pressed:

```
0x00 - None
0x01 - Button 1 (LP)
0x02 - Button 2 (RP)
0x04 - Button 3 (LK)
0x08 - Button 4 (RK)
0x10 - Heat
0x20 - Special Style
0x40 - Rage Art
```

---

## Data Structure

```cpp
// Option 1: Split fields
struct tk_input
{
    uint32_t direction;
    uint32_t button;
};

// Option 2: Combined field
struct tk_input
{
    uint64_t command;
};
```
