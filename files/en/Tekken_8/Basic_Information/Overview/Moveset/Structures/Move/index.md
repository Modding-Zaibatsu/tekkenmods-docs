# Moves
Anything that involves an animation is categorized as a "Move." This includes intros, win poses, attacks, taunts, throws, reactions to throws, Rage Arts, Heat Smashes, real-time story mode cutscenes, and stage transition animations. Each and every one of these is considered a "Move."

- Data about the body's movement makes an `animation`
- `animation` plus hitbox, hurtbox, active frames, hit conditions and cancels makes up a `move`

### Consists of
- Move Name/Key
- Animation Name/Key
- Hit level (High, mid, low, unblockable)
- Hurtbox / Vulnerability
- List of [Cancels](../Cancel/)
- List of [Hit Conditions](../Hit_Conditions/)
- List of Hit/Block sounds (optional)
- List of [Extra Move Properties](../Extra_Move_Property/) (optional)
- List of [Move Start Properties](../Move_Start_End_Property/) (optional)
- List of [Move End Properties](../Move_Start_End_Property/) (optional)
- List of Hitbox Values (optional, non-attack moves don't have hitbox values)


### A Move's corelation to other resources
```
Move
  ├─ Cancels
  │   ├─ Cancel 1
  │   │   ├─ Group Cancels
  │   │   │   ├─ Group Cancel 1
  │   │   │   ├─ Group Cancel 2
  │   │   │   └─ Group Cancel 3
  │   │   ├─ List of Requirements
  │   │   └─ Cancel Extradata
  │   ├─ Cancel 2
  │   │   ├─ Input Sequence
  │   │   │   ├─ Input 1
  │   │   │   ├─ Input 2
  │   │   │   └─ Input 3
  │   │   ├─ List of Requirements
  │   │   └─ Cancel Extradata
  │   └─ Cancel 3
  │       ├─ List of Requirements
  │       └─ Cancel Extradata
  ├─ Hit Conditions
  │   ├─ Hit Condition 1
  │   │   ├─ List of Requirements
  │   │   └─ List of Reactions
  │   │       └─ Pushback
  │   │           └─ Pushback Extradata
  │   └─ Hit Condition 2
  ├─ Voice Clips
  │   ├─ Voiceclip 1
  │   │   ├─ Folder ID
  │   │   └─ Voice ID
  │   └─ Voiceclip 2
  │       ├─ Folder ID
  │       └─ Voice ID
  ├─ Extra Move Properties
  │   ├─ Property 1
  │   │   ├─ List of Requirements
  │   │   ├─ Property ID
  │   │   └─ Frame: 0x4XXX (constant execution)
  │   ├─ Property 2
  │   │   ├─ List of Requirements
  │   │   ├─ Property ID
  │   │   └─ Frame: 0x8XXXX (instant execution)
  │   ├─ Property 3
  │   │   ├─ List of Requirements
  │   │   └─ Property ID (Can invoke a Throw Camera resource)
  │   │       └─ Throw
  │   │           └─ Throw Extradata
  │   ├─ Property 4
  │   │   ├─ List of Requirements
  │   │   └─ Property ID (Can invoke a Projectile resouce)
  │   │       └─ Projectile
  │   └─ Property 5
  │       ├─ List of Requirements
  │       └─ Property ID (Can invoke a Dialogue resource)
  │           └─ Dialogue
  ├─ Move Start Properties
  │   ├─ Start Property 1
  │   │   ├─ List of Requirements
  │   │   └─ Property ID
  │   └─ Start Property 2
  │       ├─ List of Requirements
  │       └─ Property ID
  ├─ Move End Properties
  │   ├─ End Property 1
  │   │   ├─ List of Requirements
  │   │   └─ Property ID
  │   └─ End Property 2
  │       ├─ List of Requirements
  │       └─ Property ID
  └─ Move Hitbox
      ├─ Hitbox 1
      ├─ Hitbox 2
      ├─ Hitbox 3
      ├─ Hitbox 4
      ├─ Hitbox 5
      ├─ Hitbox 6
      ├─ Hitbox 7
      └─ Hitbox 8

```
