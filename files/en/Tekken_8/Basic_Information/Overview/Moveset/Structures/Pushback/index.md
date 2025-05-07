# Pushback
This resource is used in [Reaction-Lists](../Reaction_List/) to apply pushback to opponents when an attack connects. Before diving into the structure and functionality of pushback, here are some key concepts to understand:

### Pushback Overview

A pushback is divided into two distinct phases:

1. **Non-linear Phase**  
   - This occurs at the start of the pushback.  
   - Refers to the [Pushback Extradata](../Pushback_Extradata/) resource
   - The amount of pushback applied on each frame differs (generally decreasing overtime).
   - Example:  
     - Frame 1: 100 units  
     - Frame 2: 50 units  
     - Frame 3: 25 units  
     - ...and so on

2. **Linear Phase**  
   - This follows the non-linear phase.  
   - Pushback is applied at a constant rate per frame.


This structure consists of:
- Linear Displacement: Duration
- Linear Displacement: Units (how much distance is covered per frame)
- Number of non-linear Pushback Items ([Pushback Extradata](../Pushback_Extradata/))
- Non-linear Pushback Items ([Pushback Extradata](../Pushback_Extradata/))


### Example

Let's take an example `Pushback` item from Nina's moveset
```
Linear Duration: 44
Linear Displacement: 60
Num of non-linear Pushback Items: 8
non-linear Pushback Items: [132, 61, 30, 15, 10, 0, 0, 0]
```
- This has `8` frames of non-linear pushbacks ([Pushback Extradata](../Pushback_Extradata/)) where first frame will have `132` units of displacement, second will be `61`, third will `15`, fourth will be `10` and the rest will be `0`.
- After the first `8` frames, over the course of next `44` frames, a linear displacement of `60` will be applied.

### Structure
<details>
  <summary>All Games (Post-Tekken 5)</summary>

```cpp
struct tk_pushback
{
  uint16_t duration; // Linear
  int16_t displacement; // Linear (can be +ve or -ve)
  uint32_t num_of_pushback_extradatas; // How many non-linear frames
  tk_pushback_extradata *pushback_extradata; // Array of non-linear displacements
};
```
</details>
<br/>
