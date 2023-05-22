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

**NOTE**: Some stage files start with `T7_`.

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

## Asset Prefixes
 - SK – Skeleton Mesh
 - SKT – Skeleton
 - SM – Static Mesh
 - BP – Blueprint
 - EI – Extra Item (Sets)
 - ECI – Effect Item
 - ACI – Aura Item
 - PA – Physics Asset
 - CS – Character Set
 - M – MasterShader
 - MI – MaterialInstance
 - CCI – CustomizeCharacterItem
 - CI – Character Item
 - T – Texture

## Character Names

 - AKI – Armor King
 - ANN – Anna
 - ARB – Shaheen
 - ASA – Alisa
 - ASK – Asuka
 - BOB – Bob
 - BRY – Bryan
 - BS7 – Devil Kazumi (Boss)
 - CRZ – Gigas
 - DEK – Practice Dummy
 - DNC – Lucky Chloe
 - DRA – Dragunov
 - DVJ – Devil Jin
 - EDD – Eddy
 - ELZ – Eliza
 - FEN – Feng
 - FRV – Master Raven
 - GAN – Ganryu
 - HEI – Heihachi
 - HWO – Hwoarang
 - ITA – Claudio
 - JA4 – Jack-4
 - JA6 – Jack-6
 - JAC – Jack-7
 - JIN – Jin
 - JUL - Julia
 - KAZ – Kazuya
 - KIN – King
 - KNM - Kunimitsu
 - KUM – Kuma
 - KZM – Kazumi
 - LAR – Lars
 - LAW – Law
 - LEE – Lee
 - LEI – Lei
 - LEO – Leo
 - LIL – Lili
 - LTN – Katerina
 - MAR – Marduk
 - MIG – Miguel
 - MRX – Akuma
 - MRY – Geese
 - MRZ – Noctis
 - MUT – Josie
 - NIN – Nina
 - NSA – Negan
 - NSB – Leroy
 - NSC – Fahkumram
 - NSD – Lidia
 - PAN – Panda
 - PAU – Paul
 - STE – Steve
 - XIA – Ling Xiaoyu
 - YHE – Young Heihachi
 - YKZ – Young Kazuya
 - YOS – Yoshimitsu
 - ZAK – Tekken Force Soldier
 - ZAF - Zafina

## Stage File Prefixes

 - stg00 – Mishima Dojo
 - stg00sty – Story version of Mishima Dojo (Heihachi verus Akuma)
 - stg01 – Forgotten Realm
 - stg02 – Jungle Outpost
 - stg03 – Arctic Snowfall
 - stg04 – Twilight Conflict
 - stg05 – Dragons Nest
 - stg06 – Souq
 - stg06sty - Story version of Souq
 - stg07 – Devil’s Pit
 - stg08 – Mishima Building Elevator
 - stg08a – Mishima Building Top
 - stg09 – Abandoned Temple
 - stg10 – Duomo Di Sirio
 - stg11 – Arena
 - stg12 – G. Corp Helipad (Day)
 - stg12a – G. Corp Helipad (Night)
 - stg13 – Undamaged Mishima Dojo (Heihachi versus Kazumi)
 - stg13a – Undamaged Mishima Dojo (Kazuya & Heihachi versus Jack-5s)
 - stg14 – Brimstone and Fire
 - stg15 – Precipice of Fate
 - stg16 – Violet Systems
 - stg16sty – Violet Systems (Story Mode - Lee versus Alisa)
 - stg17 – Deleted stage, includes assets of stgChr_cust
 - stg18 – Violet Industries Hallway (Story stage)
 - stg19 – Kinder Gym
 - stg20 – Infinite Azure
 - stg21 – Geometric Plane
 - stg22 – Warm Up
 - stg23 – Howard Estate
 - stg24 – Hammerhead
 - stg26 – Jungle Outpost 2
 - stg27 – Twilight Conflict 2
 - stg28 – Last Day on Earth
 - stg29 – Infinite Azure 2
 - stg30 – Cave of Enlightenment
 - stg31 - Vermilion Gates
 - stg32 - Island Paradise
 - stg80 – Infinite Azure 2 (PlayStation 4 VR Stage)
 - stg81 – Infinite version of Devil’s Pit
 - stg90 – Bowling Alley
 - stgChr_cust – Customization Stage
 - stgChr_view – The color changing part of the Customization stage
 - WallStage – Developer test stage

## Stage Suffix Names
 - stg??_debug – Include debug information used by the developers
 - stg??_demo – Can include various files
 - stg??_edit – Can include various files
 - stg??_effect – Adds effects to a map/stage
 - stg??_geom – Main map/stage files that include the geometry/mesh data
 - stg??_light – The map/stage light
 - stg??_lightstatic – Can include light-information to brighten map/stage
 - stg??_mob – Can include various files
 - stg??_reload – Include destructible meshes
 - stg??_sound – Include sound effects
 - stg??_WB - Wall brush

## File Locations
Character assets: `TekkenGame\Content\Characters`

Stages: `TekkenGame\Content\Maps`

Stage assets: `TekkenGame\Content\Stage`

UI assets: `TekkenGame\Content\UI_common`

Sound assets: `TekkenGame\Content\WwiseAudio`