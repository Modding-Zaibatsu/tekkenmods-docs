# **Throw Camera**

The **Throw Camera** resource is used to play cinematic camera animations during a **throw attack**. It is activated via an associated [Extra Move Property](../Extra_Move_Property/).

Each `Throw Camera` entry defines a **pool of camera options**, from which **one is randomly selected** and applied during the throw sequence.

---

## **Components**

A `Throw Camera` consists of the following:
* End Side
* [Camera Extra Data](../Camera_Extra_Data/) List

---

### **End Side**

Defines where the camera ends after the throw:

| Value | Meaning              |
| ----- | -------------------- |
| `0`   | Same Side            |
| `1`   | Opposite Side        |
| `2`   | Either Side (Random) |

---

### **Camera Extra Data List**

This is a list of [Camera Extra Data](../Camera_Extra_Data/) entries:

* The game will randomly choose one of the listed entries during the throw.
* The list **terminates** when an entry with **all zero values** is encountered.

This design allows for variability in throw cinematics while preserving control over the available options.

---

## **Data Structure**

<details>
  <summary><strong>Applicable to Tekken 5 and later</strong></summary>

```cpp
struct tk_throw_extra
{
  uint16_t pick_probability;         // Typically 1 or 2
  uint16_t camera_type;              // 15 = Animation Cam, 16 = Customization Cam
  uint16_t left_side_camera_data;    // Index or pointer to left-side camera data
  uint16_t right_side_camera_data;   // Index or pointer to right-side camera data
  uint16_t additional_rotation;      // Extra rotation applied to the camera
};

struct tk_throw
{
  uint64_t end_side;                 // Where the camera ends (0 = same, 1 = opposite, 2 = any)
  tk_throw_extra* camera_data;       // Pointer to list of camera extras
};
```

</details>

---

## **Version Note**

> In **Tekken 8 v2.00.01**, all characters (except "Dummy") share the same 21 `Throw Camera` entries at the start of their list.
