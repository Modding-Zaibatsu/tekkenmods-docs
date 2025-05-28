# Cancel

In gameplay terms, moves can be visualized as nodes in a graph, and **cancels** are the edges connecting them. A cancel determines how and when a move transitions into another move, based on specific inputs and conditions.

Every move includes at least one cancel. A cancel list ends with a command value of `0x8000`.

---

## Structure Overview

A cancel resource consists of the following components:

* **Command**
* **List of [Requirements](../Requirement/)**
* **[Cancel Extra Data](../Cancel_Extra_Data/)** (optional properties)
* **Input Detection Window (Start)**
* **Input Detection Window (End)**
* **Cancel Frame**
* **Move ID**
* **Cancel Type**

---

## Where Cancels Are Used

* [Moves](../Move/)
* [Projectiles](../Projectile/)

---

## Structure Definition

```cpp
struct tk_cancel
{
    uint64_t command;
    tk_requirement* requirements;
    tk_cancel_extradata* extradata;
    uint32_t input_window_start;
    uint32_t input_window_end;
    uint32_t starting_frame;
    uint16_t move_id;
    uint16_t type;
};
```

> This structure has remained consistent since **Tekken 6**.

---

## Cancel Types

The `type` field determines how a cancel behaves. Types include:

1. **Command-Based Cancel with Requirements:** Cancels to a move when both the input command and requirements are met.
2. **Condition-Based Cancel Only:** Cancels to a move when requirements alone are satisfied (no input needed).
3. **Trigger Extra Properties (No Cancel):** Executes extra behavior or properties without changing the move, requires input
4. **Trigger Extra Properties Based on Requirements Only:** Same as above, but without requiring input.

---

## Cancel – Command

The `command` field specifies the input required to trigger the cancel. It consists of:

* **Directional Input** (e.g., `forward`, `down-forward`, `neutral`)
* **Button Input** (e.g., `LP`, `RP`, `Rage Art`, `Heat`)

For detailed breakdowns, refer to the [Input Extradata](../Input_Extradata/) resource.

### Special Command Values

Cancels may refer to external resources using pre-defined command values:

| Value     | Meaning                                         |
| --------- | ----------------------------------------------- |
| `0x8000`  | End of cancel list                              |
| `0x8001`  | Double Tap Forward                              |
| `0x8002`  | Double Tap Back                                 |
| `0x8003`  | Double Tap Forward (alternate)                  |
| `0x8004`  | Double Tap Back (alternate)                     |
| `0x8005`  | Double Tap Up                                   |
| `0x8006`  | Double Tap Down                                 |
| `0x8012`  | Start of Group Cancel (Tekken 8 v2.01)          |
| `0x8013`  | End of Group Cancel (Tekken 8 v2.01)            |
| `0x8014+` | Input Sequence ID (e.g., `0x8014` = Sequence 0) |

---

## Cancel – Requirements

These are the conditions that must be met for the cancel to trigger. See [Requirement](../Requirement/) for details.

---

## Cancel – Cancel Extra Data

Used to specify additional behavior during the cancel transition. For example:

* **Frame Retention:**
  Allows a move to retain its current frame when canceling.

  * Example: In Kazuya’s Hellsweep (`CD+4`), the move visually looks identical whether it's standalone or part of `CD+4,1`. But internally, they’re separate moves. Using a `Cancel Extra Data` value of `1024` ensures the frame continuity between the two.

* **Airborne Cancels:**
  Prevents the game from resetting the character’s position to the ground.

  * Used in airborne moves like Akuma’s air flips or Clive’s jumping attacks.
  * Requires `Cancel Extra Data` value of `128`.

* **Throw Mechanic Triggers:**
  Attaches both characters together and simultaneously plays the action and reaction animations on the player and the opponent respectively. The reactions are applied from [Hit Condition](../Hit_Conditions/).

  * Requires value `6876` (`0x1ADC`).

More information is available in [Cancel Extra Data](../Cancel_Extra_Data/).

---

## Cancel – Input Detection Window

Specifies the timeframe during the current move in which the cancel input can be registered.

* `input_window_start`: The starting frame (inclusive).
* `input_window_end`: The ending frame (inclusive).

**Example:**
If start = 10 and end = 27, you have 17 frames to input the cancel.

A value of `32767` indicates that the input is valid during the entire move duration.

---

## Cancel – Cancel Frame

Specifies the exact frame at which the cancel is applied. If this is set to `0` along with input detection values, it means the cancel occurs at the end of the animation.

**Example:**
EWGF (Electric Wind God Fist) has a 50-frame animation, but cancels back to idle at frame 36.

---

## Cancel – Move ID

Defines the destination move to transition into when the cancel occurs.

### Special Cases:

* **`Move ID >= 32768` (`0x8000`)**
  Refers to **generic moves**.

  * `32769` → Default standing stance
  * `32770` → Default crouching stance
    Refer to the **Generic IDs** tab in your spreadsheet.

* **Group Cancels (`command == 0x8012`)**

  * `move_id` points to the **starting index** in the group cancel array.
  * Cancels are processed sequentially until `0x8013` is encountered.

* **`move_id == 0`**
  Indicates a self-looping cancel — the move continues to cancel into itself.

---

## Cancel – Cancel Type

The `type` value determines the nature of the cancel:

* **Even values:** Manual cancels (require player input)
* **Odd values:** Automatic cancels (triggered only by conditions)

This distinction allows the engine to differentiate between player-controlled and system-triggered transitions.
