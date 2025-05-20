# Requirement

Numerical values represent specific conditions that must be satisfied for the parent entity to take effect. These conditions, known as requirements, are frequently used in various contexts. Some requirements depend on specific parameter values, while others do not. The requirements are evaluated sequentially, and if any condition fails, the evaluation stops immediately.

Requirements start from `0` and go up to around `1100` (for Tekken 8), whereas property values begin at `0x8001`. This distinction between Requirements and [Extra Move Properties](../Extra_Move_Property/) is crucial.

For example (all values are according to Tekken 8 v2.00.03):
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
- [Cancels](../Cancel/)
- [Hit Conditions](../Hit_Conditions/)
- [Dialogues](../Dialogue_Manager/)
- [Extra Move Properties](../Extra_Move_Property/)
- [Move Start/End Properties](../Move_Start_End_Property/)

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
- If a requirement value is `>= 0x8001` then it's not a Requirement, it's an [Extra Move Property](../Extra_Move_Property/).
- If instead of a requirement value, the list stores an [Extra Move Property](../Extra_Move_Property/), it will simply be executed/applied on the current move. In the above example, the list stores 2 Properties "Play Sound" and "Camera Shake", those will be applied to the current move as soon as the hit lands on an upright opponent.

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
<br/>
