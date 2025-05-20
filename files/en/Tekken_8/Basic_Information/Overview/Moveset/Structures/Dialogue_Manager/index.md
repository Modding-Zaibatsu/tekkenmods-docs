# Dialogue Manager

The **Dialogue Manager** is a new system introduced in *Tekken 8* to control character dialogue and facial animations during interactions with specific opponents. It standardizes how `intro` and `win pose` animations are handled, giving each character:

* **3 intro animations**
* **3 win pose animations**
  (*Not including Fate intros*)

These animations are triggered via the [Extra Move Property](../Extra_Move_Property/) using the following properties:

* `0x877B` — Triggers **facial animation**
* `0x87F8` — Triggers a **voice clip**

---

### Components

Each entry in the Dialogue Manager consists of:

* `Type` — Specifies the context (Intro, Outro, or Fate Intro)
* `Cinematic ID` — Identifies the specific animation sequence
* `Requirements` — List of [Conditions](../Requirement/) that must be met for the dialogue to trigger
* `Voiceclip ID` — Encrypted or transformed identifier for the voice line
* `Facial Animation ID` — Index of the facial animation

---

### How It Works

Each character has three standardized intros and three win poses. These are defined using the `Type` and `Cinematic ID` fields:

```
TYPE   CINEMATIC ID   DESCRIPTION
0      N              Nth Intro (values: 0–2)
1      N              Nth Outro (values: 0–2)
2      XY             Fate Intro:
                      X = Segment (0–2)
                      Y = Player (0) or Opponent (1)
```

#### Examples:

* `Type = 0`, `Cinematic ID = 0` → 1st Intro
* `Type = 1`, `Cinematic ID = 0` → 1st Outro
* `Type = 2`, `Cinematic ID = 10` → Fate Intro, Segment 1, Player animation
* `Type = 2`, `Cinematic ID = 21` → Fate Intro, Segment 2, Opponent animation

Fate intros involve multiple segments (X = 0–2), and both characters have animations playing simultaneously. `Y = 0` means it's the player’s animation; `Y = 1` means it’s the opponent’s.

Note: A voiceclip having a value of `0x88E3EE99` seems to mean silence or "NO VOICECLIP"

---

### Example: Leo's Dialogue Data

```
TYPE     CINEMATIC_ID   VOICECLIP     FACIAL_ANIM_ID   REQUIREMENTS
Intro    1              0x9DC0A576    114              Episode Pre-fight, Opponent: RAVEN
Intro    2              0x4AB13EA2    115              Episode Pre-fight, Opponent: YOSHIMITSU
Outro    1              0xCA9173D2    117              Episode Post-fight, Opponent: AZUCENA
Intro    0              0x12DDC46D    124              None
Intro    1              0x07442268    125              None
Intro    2              0x06FFE5A7    116              Condition 804:1, Opponent: KAZUYA
Intro    2              0xBA7FFD01    126              None
Outro    0              0x8A5135E2    128              None
Outro    1              0x783B6486    118              Opponent: AZUCENA
Outro    1              0x733B937A    129              None
Outro    2              0xFCC3BECE    130              None

// Fate Intro – Chapter 10 Story Battle
Fate     0              0x88E3EE99    131              Story Fight: Chapter 10 Battle 4
Fate     1              0x1013D932    119              Story Fight: Chapter 10 Battle 4
Fate    10              0x84216BBB    120              Story Fight: Chapter 10 Battle 4
Fate    11              0x88E3EE99    134              Story Fight: Chapter 10 Battle 4
Fate    20              0x17B414F3    135              Story Fight: Chapter 10 Battle 4
Fate    21              0xC5688173    136              Story Fight: Chapter 10 Battle 4

// Fate Intro – Leo vs Azucena
Fate     0              0x88E3EE99    131              Controlled Character: LEO, Opponent: AZUCENA
Fate     1              0x3D3B53E8    132              Controlled Character: LEO, Player: AZUCENA
Fate    10              0x66DDD077    133              Controlled Character: LEO, Opponent: AZUCENA
Fate    11              0x88E3EE99    134              Controlled Character: LEO, Player: AZUCENA
Fate    20              0x17B414F3    135              Controlled Character: LEO, Opponent: AZUCENA
Fate    21              0xC5688173    136              Controlled Character: LEO, Player: AZUCENA
```

---

### Data Structure

```cpp
struct tk_dialogue {
  uint16_t type;              // 0 = Intro, 1 = Outro, 2 = Fate Intro
  uint16_t cinematic_id;
  uint32_t _0x4;              // Unused/reserved
  tk_requirement* requirements;
  uint32_t voiceclip;         // Encrypted/converted voiceclip ID
  uint32_t facial_anim_idx;   // Index of facial animation
};
```
