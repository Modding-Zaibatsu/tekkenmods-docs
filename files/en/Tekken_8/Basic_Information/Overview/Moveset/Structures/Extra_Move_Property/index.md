# Extra Move Properties
As the name implies, these are optional properties that can be added to a move. While a Move resource includes its Hitbox and Hurtbox values, other elements—such as modifying flags, controlling hand or facial gestures, consuming tornado effects, or triggering audio and visual effects—are managed through these lists.

Property values starts from `0x8001`. That's a crucial difference between requirements and properties. Requirements start from `0` and go till around `1100` (for Tekken 8)

### Consist of
- Frame Number
- List of Requirements (Starting from Tekken 8)
- Property ID
- 5 Parameter values (if any) - Prior games only had 1 possible parameter

Among other things, an Extra Move Property can invoke other moveset resources such as:
- Spawn a [Projectile](../Projectile/)
- Use a [Throw Camera](../Throw_Camera/)
- Utilize a [Dialogue](../Dialogue_Manager/) during an intro/outro
- Apply a [Pushback](../Pushback/) on self or opponent
- Force a [Move](../Move/) on self or opponent

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
