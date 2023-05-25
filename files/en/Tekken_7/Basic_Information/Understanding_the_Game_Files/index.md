## Breaking down file names
The files in Tekken 7 contain letters that describe it's type. For example:
`T_CH_kaz_bdu_1P_wind_coat_D`

Let's break down the file name.

`T` - This prefix stands for Texture

`CH` - This indicates that it's a file relates to a character

`kaz` - Indicates that this is Kazuya's texture

`1P_wind_coat` - The item this file is related to

`D` - In the context of a texture file, this indicates that this is the diffuse texture map.

Not every asset may follow the same naming scheme and it varies between asset types, whether it's for a character or a stage, a blueprint, etc.

**INFO**: Some stage files start with `T7_`.

Let's break down a few more asset file names:

`T7_MI_ENV_s8BuildingNearAConc`

`T7` - Stands for TEKKEN 7

`MI` - MI means that the asset file is a Material Instace

`s8BuildingNearAConc` - The item this file is related to, s8 indicates that it's for stage 8 - Mishima Building Elevator.

---

`M_CH_Base`

`M` - This prefix stands for Material

`CH` - This incidates that it's a file relates to a character

`Base` - In the context of the file, this is the base clothing material file for characters.

---

`M_CH_Aniso_mask_HueShift`

`M` - This prefix stands for Material

`CH` - This incidates that it's a file relates to a character

`Aniso` - Stands for `Anisotropic`. In the context of this file, this is a material that is anisotropic.

`mask` - Indicates that the material is of a mask type, which means that it contains transparency

`HueShift` - An additional suffix that indicates that this material contains a hue shift that interpolates between different colors.


## File Locations
Character assets: `TekkenGame\Content\Characters`

Stages: `TekkenGame\Content\Maps`

Stage assets: `TekkenGame\Content\Stage`

UI assets: `TekkenGame\Content\UI_common`

Sound assets: `TekkenGame\Content\WwiseAudio`