# Cancel
If you imagine moves as nodes in a graph, then cancels are the edges connecting them. Cancel resources define how and why a move transitions into another move.
A cancel occurs only when the necessary commands and conditions are met. Each move has at least one cancel, and the end of a cancel list is marked by a cancel with a command value of `0x8000`.
<br/>
### Consists of
- Command
- List of [Requirements](../Requirement/)
- [Cancel Extra Data](../Cancel_Extra_Data/) (Additional cancel properties)
- Input Detection Window (Start)
- Input Detection Window (End)
- Cancel Frame
- Move ID
- Cancel Type

### List of cancels Found in
- [Moves](../Move/)
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
- Cancel into a move by Command and meeting [Requirements](../Requirement/) (if any)
- Cancel into a move by meeting [Requirements](../Requirement/) (if any)
- No canceling, invoke [Extra Properties](../Extra_Move_Property/) upon meeting Command and [Requirements](../Requirement/) (if any)
- No canceling, invoke [Extra Properties](../Extra_Move_Property/) upon meeting [Requirements](../Requirement/) (if any)

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
List of conditions that need to be fulfilled for the cancel to happen. This could also be a list of properties that need to be executed based on some conditions. Refer to the [Requirement](../Requirement/) resource.

### Cancel - Cancel Extra Data
Additional properties to apply on the cancel. Description of the resouce is [below](../Cancel_Extra_Data/)
- Sometimes, you may want to cancel into another move while retaining your current frame count. Let me explain with an example: Kazuya's Spinning Demon (or "Hellsweep") [CD+4]. On its own, CD+4 never knocks down the opponent. However, if you follow it with CD+4,1 or CD+4,4, you'll notice the first hit causes the opponent to trip. Why is this? Behind the scenes, there are two visually identical moves for CD+4, differing only in their hit reactions. Let’s call the standalone CD+4 `Move A`, and the initial part of CD+4,1 or CD+4,4 `Move B`. When you input CD+4 on its own, `Move A` is performed. But if you press 1 or 4 during the move, it transitions seamlessly into `Move B` without any visual difference. What makes this transition smooth is a mechanism called `Cancel Extra Data`. If the value is set to `1024`, it ensures the frames of the current move carry over during the transition. For example, if you press 1 during the 10th frame of CD+4, the game transitions from `Move A` to `Move B` while keeping the frame count at 10. This means `Move B` starts from the same frame instead of resetting to frame 1. This frame continuity is what enables the smooth transition between the two moves.
- For airborne cancels, like Akuma's air-flips from Tekken 7 or Clive's jump moves from Tekken 8, the game, by default puts the character on the ground when a new move starts. But it's prevented by having a `Cancel Extra Data` value of `128`.
- The way throws work, both the action & the reaction animations are played on the character & the opponent simultaneously while the distance b/w them is decreased to 0. That happens when Cancel Extradata is `6876 (0x1ADC)`. It picks the reaction animation from the [Hit Condition](../Hit_Conditions/) and applies it to the opponent.

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
<br/>
