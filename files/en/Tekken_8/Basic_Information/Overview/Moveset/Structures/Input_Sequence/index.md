# Input Sequence

This resource is invoked in [Cancels](../Cancel/) to check if a user has performed a sequence of inputs or not.

---

### Components

Each entry in the Input Sequence consists of:

* `Window` — Duration of the input window in frames
* `Count` - Number of inputs within the sequence
* [List of Inputs](../Input_Extradata/)

---

### How It Works
Each game has a commad value that's used to dictate the start of the `Input Sequence` array, in Tekken 8 v2.01, this value is `0x8014`.
- `0x8014` -> 1st sequence
- `0x8015` -> 2nd sequence
- `0x8016` -> 3rd sequence
- ... so on

When the game encounters an input sequence, the input buffer is checked to see if the player has performed given inputs within the past X frames.

As an example, let's take Bryan's Jet Upper command, the input sequence number is 254, which means the command for this will be `0x8014` + `254` = `0x8112`.
```
Window: 40
Amount: 4
Inputs: n > f > n > b+2
```
For this sequence, the game checks if the above mentioned sequence is in the input buffer, If there's any additional input b/w the 2 required inputs, it is simply ignored.
- For example, the player can input `n > f > df > d > db > b > n > b+2` and it would still be accepted because the game was able to find the `n > f > n > b+2` sequence.
- Imagine the user enters this sequence, `n > f > b+2`, in this case, the Jet Upper will not come out since we're missing a neutral input before `b+2` (or aftr `f`)

---

### Data Structure

```cpp
struct tk_input
{
  // can further be broken down to "button" & "direction"
  uint64_t command;
};


struct tk_input_sequence
{
  uint16_t input_window_frames;
  uint16_t input_amount;
  uint32_t _0x4;
  tk_input *inputs;
};
```