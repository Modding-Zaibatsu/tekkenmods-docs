# Move Start/End Properties
This resource is similar to [Extra Move Properties](../Extra_Move_Property/) but with one key difference: it does not include a "Starting Frame" attribute. As the name suggests, a move can have a list of properties assigned to it, which are executed either just before the move begins or after it ends. The list continues until it encounters the "End of List" requirement, marking its conclusion.

### Consists of
- List of Requirements
- Property ID
- 5 Parameter values (if any)

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

struct tk_start_end_props
{
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

struct tk_start_end_props
{
  tk_requirement *requirements;
  uint32_t property;
  tk_param param;
};
```
</details>
<br/>
They look like this in memory in hexadecimal view:

```
<PTR to Requirements> | 000084CC | 00000010 00000020 00000000 00000000 00000000
<PTR to Requirements> | 000082A5 | 0000000A 00000000 00000000 00000000 00000000
<PTR to Requirements> | 0000044C | 00000000 00000000 00000000 00000000 00000000
```

Property `1100` (0x44C) denotes the end of the list
<br/>
