# Introduction
This article presents basic information about common resources that you see in Tekken Movesets. Bandai Namco has been using the same animation engine that they've been using since Tekken 1 but with changes built on top of it in each game. I suggest having this [spreadsheet](https://docs.google.com/spreadsheets/d/1DBkC-HfqD0KWQNeOTKjJWmPxdbEuCcGZxkPxQpsLkOY/edit?usp=sharing) open as you explore this article.

Huge thanks to `Sadamitsu` who explained some of the things I wasn't clear about and `Dennis` for proof-reading the documentation.

### Please Note
The structures described here are based on the latest game, *Tekken 8*. While there may be minor differences in structures across different games in the series, please note that these explanations are specifically tailored for said game. You can still get a decent idea for how Movesets work for prior games through this documentation.

A `Moveset` is made up of several structures, each represented as a list. These structures are interconnected through references, where one element typically points to a range of others. The end of such a reference is determined by a specific value within the structure—this acts as a boundary. For example, `Move` #100 may reference `Cancel` #3581 through #3589, and a value within the `Cancel` structure defines where this connection ends. In general, these relationships are one-to-many but can be one-to-one too. For instance, a `Move` references multiple Cancels, and each `Cancel` in turn may reference its own set of Requirements and an associated `Cancel Extra Data`.

Movest Structures are listed below:

  - [Moves](./Structures/Move/)
  - [Cancels](./Structures/Cancel/)
  - [Group Cancels](./Structures/Group_Cancel/)
  - [Cancel Extra Datas](./Structures/Cancel_Extra_Data/)
  - [Requirements](./Structures/Requirement/)
  - [Hit Conditions](./Structures/Hit_Conditions/)
  - [Reaction Lists](./Structures/Reaction_List/)
  - [Pushbacks](./Structures/Pushback/)
  - [Pushback Extradatas](./Structures/Pushback_Extradata/)
  - [Extra Move Properties](./Structures/Extra_Move_Property/)
  - [Move Start/End Properties](./Structures/Move_Start_End_Property/)
  - [Parrable Moves List](./Structures/Parryable_Move/)
  - [Voiceclips](./Structures/Voiceclip/)
  - [Input Sequences](./Structures/Input_Sequence/)
  - [Input Extradata](./Structures/Input_Extradata/)
  - [Projectiles](./Structures/Projectile/)
  - [Throw Cameras](./Structures/Throw_Camera/)
  - [Camera Extra Datas](./Structures/Camera_Extra_Data/)
  - [Dialogue Managers](./Structures/Dialogue_Manager/)


### Relationships
#### Move
- A `Move` must have atleast one `Cancel`, can have multiple
- A `Move` must have atleast one `Hit Condition`, can have multiple
- A `Move` can optionally have multiple `Extra Move Properties`
- A `Move` can optionally have multiple `Move Start/End Properties`
- A `Move` can optionally have multiple `Voiceclips`
- A `Move` can optionally have multiple `Hitboxes`
- A `Move` can optionally have an entry in the `Parrable Moves List` array

### Cancel / Group Cancel
- A `Cancel` must have atleast one `Requirement`, can have multiple
- A `Cancel` can have only one `Cancel Extra Data`
- A `Cancel` can refer to only a single Move (by it's index)
- A `Cancel` can reference a `Group Cancel` - **Not applied to a Group Cancel**
- A `Cancel` can reference an `Input Sequence`
- A `Cancel` must belong to a `Move` or a `Projectile` - **Not applied to a Group Cancel**
- A `Group Cancel` must belong to a `Cancel` - **Not applied to a Regular Cancel**

### Extra Move Property (EMP)
- An EMP must have atleast one `Requirement`, can have multiple (starting from Tekken 8)
- An EMP can reference a `Projectile`
- An EMP can reference a `Throw Camera`
- An EMP can reference a `Dialogue`
- Always belong to a `Move`

### Start/End Properties
- Exact same as `Extra Move Property`

### Hit Condition (HC)
- A HC always belongs to a `Move` or a `Projectile`
- A HC must have atleast one `Requirement`, can have multiple
- A HC must have a `Reaction List`

### Reaction List (RL)
- A RL always belongs to a `Hit Condition`
- A RL always has 7 `Pushback` items
- A RL always references 17 `Moves` (by their indexes). No uniqueness among them.

### Pushback
- A `Pushback` always belongs to a `Reaction List`
- A `Pushback` has atleast one `Pushback Extradata` item, can have multiple

### Pushback Extradata
- Always belongs to a `Pushback` item

### Input Sequences
- Always referenced by a `Cancel` (or a `Group Cancel`)
- has atleast one `Input Extradata` item, can have multiple

### Input Extradata
- Always belongs to an `Input Sequences`

### Dialogue (or call it "Dialogue Manager")
- Must have atleast one `Requirement`, can have multiple
- Always referenced by an `Extra Move Property` (or `Start/End Property`)

### Projectile
- Must have atleast one `Cancel`, can have multiple
- Must have atleast one `Hit Condition`, can have multiple
- Always referenced by an `Extra Move Property` (or `Start/End Property`)

### Throw Camera
- Always referenced by an `Extra Move Property` (or `Start/End Property`)
- has atleast one `Camera Extradata` item, can have multiple

### Camera Extradata
- Always referenced by a `Throw Camera`

### Parryable Moves List
- A single item is a reference to a `Move`

### Voicelcip
- Always referenced by a `Move`

### Requirement
- A single requirement is always part of a `Requirement List`
- A `Requirement List` can be referenced by:
  - Cancel
  - Hit Condition
  - Extra Move Property
  - Move Start/End Property
  - Dialogue


### Structure
<details>
  <summary>Tekken 8</summary>

```cpp
struct tk_moveset
{
  uint16_t _0x0;
  bool is_written;
  bool _0x3;
  uint32_t _0x4;
  char _0x8[4]; // "TEK"
  uint32_t _0xC;
  uint32_t *character_name_addr;    // no longer used
  uint32_t *character_creator_addr; // no longer used
  uint32_t *date_addr;              // no longer used
  uint32_t *fulldate_addr;          // no longer used
  uint16_t original_aliases[60];
  uint16_t current_aliases[60];
  uint16_t unknown_aliases[32];
  uint32_t ordinal_id1;                          // Concatenation of previous Character ID, for Kazuya (8) -> (-7 & 7)
  uint32_t ordinal_id2;                          // Concatenation of Character ID, for Kazuya (8) -> (-8 & 8)
  // MOVESET TABLE
  tk_reaction *reactions_ptr;                    // Offset: 0x168
  uint64_t _0x170;                               // No clue why it's here but it's here
  uint64_t reactions_count;                      // Offset: 0x178
  tk_requirement *requirements_ptr;              // Offset: 0x180
  uint64_t requirements_count;                   // Offset: 0x188
  tk_hit_condition *hit_conditions_ptr;          // Offset: 0x190
  uint64_t hit_conditions_count;                 // Offset: 0x198
  tk_projectile *projectiles_ptr;                // Offset: 0x1a0
  uint64_t projectiles_count;                    // Offset: 0x1a8
  tk_pushback *pushbacks_ptr;                    // Offset: 0x1b0
  uint64_t pushbacks_count;                      // Offset: 0x1b8
  tk_pushback_extradata *pushback_extradata_ptr; // Offset: 0x1c0
  uint64_t pushback_extradata_count;             // Offset: 0x1c8
  tk_cancel *cancels_ptr;                        // Offset: 0x1d0
  uint64_t cancels_count;                        // Offset: 0x1d8
  tk_cancel *group_cancels_ptr;                  // Offset: 0x1e0
  uint64_t group_cancels_count;                  // Offset: 0x1e8
  tk_cancel_extradata *cancel_extradata_ptr;     // Offset: 0x1f0
  uint64_t cancel_extradata_count;               // Offset: 0x1f8
  tk_extraprops *extra_move_properties_ptr;      // Offset: 0x200
  uint64_t extra_move_properties_count;          // Offset: 0x208
  tk_move_start_end_props *move_start_props_ptr; // Offset: 0x210
  uint64_t move_start_props_count;               // Offset: 0x218
  tk_move_start_end_props *move_end_props_ptr;   // Offset: 0x220
  uint64_t move_end_props_count;                 // Offset: 0x228
  tk_move *moves_ptr;                            // Offset: 0x230
  uint64_t moves_count;                          // Offset: 0x238
  tk_voiceclip *voiceclips_ptr;                  // Offset: 0x240
  uint64_t voiceclips_count;                     // Offset: 0x248
  tk_input_sequence *input_sequences_ptr;        // Offset: 0x250
  uint64_t input_sequences_count;                // Offset: 0x258
  tk_input *inputs_ptr;                          // Offset: 0x260
  uint64_t inputs_count;                         // Offset: 0x268
  tk_parryable_move *parryable_list_ptr;         // Offset: 0x270
  uint64_t parryable_list_count;                 // Offset: 0x278
  tk_throw_extra *throw_extras_ptr;              // Offset: 0x280
  uint64_t throw_extras_count;                   // Offset: 0x288
  tk_throw *throws_ptr;                          // Offset: 0x290
  uint64_t throws_count;                         // Offset: 0x298
  tk_dialogue *dialogues_ptr;                    // Offset: 0x2a0
  uint64_t dialogues_count;                      // Offset: 0x2a8
  // No longer used
  tk_mota *mota_0; // Anims
  tk_mota *mota_1; // Anims
  tk_mota *mota_2; // Hand
  tk_mota *mota_3; // Hand
  tk_mota *mota_4; // Face
  tk_mota *mota_5; // Face
  tk_mota *mota_6; // Wings (probably more to it)
  tk_mota *mota_7; // Wings (probablty more to it)
  tk_mota *mota_8; // Camera
  tk_mota *mota_9; // Camera
  tk_mota *mota_10; // Unknown
  tk_mota *mota_11; // Unknown
  tk_mota *start_of_odd_mota;
};
```
</details>

<details>
  <summary>Tekken 7</summary>
  
```cpp
struct tk_moveset
{
  uint16_t _0x0;
  bool is_written;
  bool _0x3;
  char _0x4[4];
  char *character_name_addr;
  char *character_creator_addr;
  char *date_addr;
  char *fulldate_addr;
  uint16_t original_aliases[56];
  uint16_t current_aliases[56];
  uint16_t unknown_aliases[32];
  uint32_t ordinal_id1; // Concatenation of previous Character ID, for Kazuya (8) -> (-7 & 7)
  uint32_t ordinal_id2; // Concatenation of Character ID, for Kazuya (8) -> (-8 & 8)
  // MOVESET TABLE
  tk_reaction *reactions_ptr;
  uint64_t reactions_count;
  tk_requirement *requirements_ptr;
  uint64_t requirements_count;
  tk_hit_condition *hit_conditions_ptr;
  uint64_t hit_conditions_count;
  tk_projectile *projectiles_ptr;
  uint64_t projectiles_count;
  tk_pushback *pushbacks_ptr;
  uint64_t pushbacks_count;
  tk_pushback_extradata *pushback_extradata_ptr;
  uint64_t pushback_extradata_count;
  tk_cancel *cancels_ptr;
  uint64_t cancels_count;
  tk_cancel *group_cancels_ptr;
  uint64_t group_cancels_count;
  tk_cancel_extradata *cancel_extradata_ptr;
  uint64_t cancel_extradata_count;
  tk_extraprops *extra_move_properties_ptr;
  uint64_t extra_move_properties_count;
  tk_move_start_end_props *move_start_props_ptr;
  uint64_t move_start_props_count;
  tk_move_start_end_props *move_end_props_ptr;
  uint64_t move_end_props_count;
  tk_move *moves_ptr;
  uint64_t moves_count;
  tk_voiceclip *voiceclips_ptr;
  uint64_t voiceclips_count;
  tk_input_sequence *input_sequences_ptr;
  uint64_t input_sequences_count;
  tk_input *inputs_ptr;
  uint64_t inputs_count;
  tk_parryable_move *parryable_list_ptr;
  uint64_t parryable_list_count;
  tk_throw_extra *throw_extras_ptr;
  uint64_t throw_extras_count;
  tk_throw *throws_ptr;
  uint64_t throws_count;
  tk_mota *mota_0; // Anims
  tk_mota *mota_1; // Anims
  tk_mota *mota_2; // Hand
  tk_mota *mota_3; // Hand
  tk_mota *mota_4; // Face
  tk_mota *mota_5; // Face
  tk_mota *mota_6; // Wings (probably more to it)
  tk_mota *mota_7; // Wings (probablty more to it)
  tk_mota *mota_8; // Camera
  tk_mota *mota_9; // Camera
  tk_mota *mota_10; // Unknown
  tk_mota *mota_11; // Unknown
  tk_mota *start_of_odd_mota;
};
```
</details>