# **Camera Extradata**

The **Camera Extradata** resource defines how the camera behaves during a throw animation. It is referenced by a [Throw Camera](../Throw_Camera/) entry and influences the cinematic experience of a throw.

---

## **Components**

Each `Camera Extradata` entry consists of the following fields:
* Pick Probability
* Camera Type
* Left Side Camera Data
* Right Side Camera Data
* Additional Rotation

---

### **Pick Probability**

* Determines the chance of this entry being selected.
* **Valid values:**

  * `1` — Normal probability
  * `2` — Twice as likely to be picked as entries with a value of `1`

---

### **Camera Type**

Specifies which kind of camera behavior to use:

| Value | Meaning                                |
| ----- | -------------------------------------- |
| `21`  | Use a predefined camera animation      |
| `22`  | Use a customized, dynamic camera setup |

---

### **Left Side / Right Side Camera Data**

The behavior of these fields depends on the **Camera Type**:

---

## **When Camera Type = 22 (Customized Camera)**

Camera values are encoded as a **4-digit hexadecimal**: `XXYZ`

### Field Breakdown:
**Note: All Values are in Hexadecimal**

| Segment                                                      | Meaning                             |
| ------------------------------------------------------------ | ----------------------------------- |
| `XX`                                                         | Usually `08`. Reserved/prefix byte. |
| `Y`                                                          | **Transition Timing**               |
|   - Even = Smooth start                                      |                                     |
|   - Odd = Instant start                                      |                                     |
|   - `Y ≥ 7` = Transition from a distance (if `XX == 08`)     |                                     |
| `Z`                                                          | **Camera Angle Preset**             |

#### `Z` Values – Angle Presets

|  Z  | Camera Angle       |
| --- | ------------------ |
| 0x0 | Mid / Right        |
| 0x1 | Mid / Center       |
| 0x2 | Mid / Left         |
| 0x3 | Mid / Center       |
| 0x4 | Bottom / Right     |
| 0x5 | Bottom / Center    |
| 0x6 | Bottom / Left      |
| 0x7 | Bottom / Center    |
| 0x8 | Top / Right        |
| 0x9 | Top / Center       |
| 0xA | Top / Left         |
| 0xB | Top / Center       |
| 0xC | Mid / Right (alt)  |
| 0xD | Mid / Center (alt) |
| 0xE | Mid / Left (alt)   |
| 0xF | Mid / Center (alt) |

---

## **When Camera Type = 21 (Animation Camera)**

* Value format: `0xXYYY`

  * `X` — Source type (typically refers to the “mota” or animation file group)
  * `YYY` — Animation index within the file

This setup directly indexes into the **Camera Animation** resource.

---

### **Additional Rotation**

* Specifies additional camera rotation **in degrees**.
* Allows fine-tuning of the camera spin or tilt during the throw sequence.

---

## **Sample Data**

Below is sample Camera Extra Data List fetched from one of Paul's throws

```
0x0001 0x0016 0x0814 0x0814 0x0000
0x0001 0x0016 0x0815 0x0815 0x0000
0x0001 0x0016 0x0816 0x0816 0x0000
0x0001 0x0016 0x0804 0x0804 0x0000
0x0001 0x0016 0x0805 0x0805 0x0000
0x0001 0x0016 0x0806 0x0806 0x0000
0x0000 0x0000 0x0000 0x0000 0x0000 (End of List)
```

* Any one of these will be picked randomly to be performed on the Throw, and the last line at the end comprising of all 0s dictates the end of the list.
* All of them have an equal chance of being picked because of value 1 in the 1st column
* All of them are Customized since the 2nd column is 22 (0x16)
* 3 of them use `Smooth Transitioning` while 3 use `Instant Transition` because of the `Y` value in 3rd and 4th columns
* They all start at `Bottom / Right`, `Bottom / Center` and `Bottom / Left` because of the `Z` value in the 3rd and 4th columns

---

## **Data Structure**

<details>
  <summary><strong>Applicable to Tekken 5 and later</strong></summary>

```cpp
struct tk_throw_extra
{
  uint16_t pick_probability;         // 1 = normal, 2 = higher chance
  uint16_t camera_type;              // 21 = animation, 22 = customized
  uint16_t left_side_camera_data;    // Depends on camera_type
  uint16_t right_side_camera_data;   // Depends on camera_type
  uint16_t additional_rotation;      // Degrees of rotation
};

struct tk_throw
{
  uint64_t end_side;                 // Where camera ends (0 = same, 1 = opposite, 2 = any)
  tk_throw_extra* camera_data;       // Pointer to camera extradata list
};
```

</details>

---
