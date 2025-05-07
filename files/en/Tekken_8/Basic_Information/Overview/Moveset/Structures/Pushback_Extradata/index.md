# Pushback Extradata
This resource dictates the distance applied on the body per-frame. Each [Pushback](../Pushback/) item has a list of Pushback Extradata attached to it. As explained above, it's used for defining non-linear pushback behaviour of a reaction.

### Structure
<details>
  <summary>All Games (Post-Tekken 5)</summary>

```cpp
struct tk_pushback_extradata
{
  int16_t displacement; // Can be both +ve and -ve
};
```
</details>
<br/>
