# **Input Sequence**

This resource is primarily used in the **[Cancels](../Cancel/)** system to verify whether a player has executed a specific sequence of inputs within a defined timeframe.

---

## **Overview**

Each **Input Sequence** consists of:

* **`Window`** — Duration (in frames) during which the sequence must be completed.
* **`Count`** — Total number of required inputs in the sequence.
* **[List of Inputs](../Input_Extradata/)** — The specific directional and button inputs needed.

---

## **How It Works**

Each game command references a specific **Input Sequence** ID. In **Tekken 8 v2.01**, the base value is:

```
Base Command Value: 0x8014
```

To compute the final command value for a move:

```
Final Command = 0x8014 + Input Sequence Index
```

### **Example: Bryan’s Jet Upper**

* **Input Sequence Index**: `254`
* **Final Command Value**: `0x8014 + 254 = 0x8112`

```yaml
Window: 40
Count: 4
Inputs:
  - n
  - f
  - n
  - b+2
```

This means the game will check if the following input pattern appears in the buffer within the past **40 frames**:

```
n → f → n → b+2
```

* **Input Flexibility**:

  * The sequence **can** contain extra inputs in between — they are **ignored** as long as the correct sequence appears in the right order.
  * ✅ Example (Accepted):

    ```
    n → f → df → d → db → b → n → b+2
    ```
  * ❌ Example (Rejected):

    ```
    n → f → b+2
    ```

    * Missing the second **neutral** (`n`) input before `b+2`.

---

## Data Structure

```cpp
struct tk_input
{
  // Represents a single input, can be broken into:
  //   - Direction
  //   - Button
  uint64_t command;
};

struct tk_input_sequence
{
  uint16_t input_window_frames;  // Time window to check for input sequence (in frames)
  uint16_t input_amount;         // Number of required inputs
  uint32_t _0x4;                 // Reserved / Unknown
  tk_input* inputs;              // Pointer to input sequence array
};
```

---

## **Key Points**

* Commands are matched by scanning recent inputs for the required sequence **in order**.
* Extra or "noise" inputs between the required inputs are **ignored**.
* Sequences **must not skip required inputs**.
* Used for cancels with complex inputs like King's chain throws or Bryan's Jet Upper

---
