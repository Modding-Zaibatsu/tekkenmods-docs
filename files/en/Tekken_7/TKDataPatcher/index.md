**WARNING**: This documentation is incomplete, you can contribute to it by clicking on the GitHub links at the bottom of this page where you'll be redirected to the documentation repository.

TKDataPatcher is a tool that allows adding custom item slots for specific characters in character customization.
By using TKDataPatcher, modders can expand the available customization options by introducing their own items and modifying existing ones.

## As a User

Whenever you add new .csv files to the directory `TEKKEN 7\TekkenGame\Content\ModData\customize_item_data`, you need to run `TKDataPatcher.exe` to patch the game's files and reflect the changes made.

If two people have the same mod with the same shop item ID, the customized items will sync online. Otherwise, the opponent will see the default outfit.

## As a Modder

1. Create a directory in `TEKKEN 7\TekkenGame\Content\ModData\customize_item_data` and give it a suitable name (e.g., "yourname").
2. Within the created directory, generate a file named `items.csv` (or any other name ending with .csv). You can have multiple .csv files within one directory.

| Field                | Value          |
| -------------------- | -------------- |
| **A**. Shop ID       | 561001         |
| **B**. packageIndex  | -1             |
| **C**. CI Reference  | nsd_bdu_1p_cus |
| **D**. Unknown       | 0              |
| **E**. charId        | 56             |
| **F**. slotId        | 9              |
| **G**. Unknown       | -1             |
| **H**. Name by ID    | 0              |
| **I**. Name of item  | NIN_CUS_008    |
| **J**. Unknown       | 0              |
| **K**. Slot Group ID | -249           |
| **L**. Unknown       | 0              |
| **M**. PlayerCus ID  | 1              |
| **N**. Slot colors   | 5              |
| **O**. Unknown       | 0              |
| **P**. Rarity        | 1              |
| **Q**. packageId     | -255           |
| **R**. Item cost     | 0              |
| **S**. Unknown       | -1             |
| **T**. Unknown       | \x00           |
| **U**. Unknown       | 0              |

```csv
561001,-1,nsd_bdu_1p_cus,0,56,9,-1,0,NIN_CUS_008,0,-249,0,1,5,0,1,-255,0,-1,\x00,0
```

## Breakdown of a CSV entry

**A**: Shop ID (Item ID)
A number that tells you what slot the item is on this is unique for every item in the game.
It has to be unique.

**B**: packageIndex (int32)
Value is set to -1

**C**: CCI Reference (fileNameOffset)
The hard reference lookup that is done to find the uasset in question.
For most items the lookup will be like this: `TekkenGame/Content/<Path>/<CHARACTER_ABBREVIATION>/<SLOT_ID_TITLE>/<CCI_REFERENCE_NAME>`

**D**: Unknown (int32)
Value is set to 0

**E**: charId (0-255)
For a list of character IDs see [Characters](../Basic_Information/Overview/Characters/)

**F**: slotId (0-255)
Defines what kind of item it is.

| Slot IDs | Slot Description | Slot ID Title | Type                   | Path                      |
| -------- | ---------------- | ------------- | ---------------------- | ------------------------- |
| 1        | Head             | HEAD          | CharacterCustomizeItem | Character/Item/Customize  |
| 2        | Hairstyle        | HAIR          | CharacterCustomizeItem | Character/Item/Customize  |
| 3        | Full Head        | FULL_HEAD     | CharacterCustomizeItem | Character/Item/Customize  |
| 4        | Hair Accessory   | HAIR_ACC      | CharacterCustomizeItem | Character/Item/Customize  |
| 5        | Glasses          | GLA           | CharacterCustomizeItem | Character/Item/Customize  |
| 6        | Face             | FACE          | CharacterCustomizeItem | Character/Item/Customize  |
| 7        | Facial Hair      | FACE_HAIR     | CharacterCustomizeItem | Character/Item/Customize  |
| 8        | Face Paint       | MAKE_UP       | CharacterItemTex       | Character/Item/Customize  |
| 9        | Upper Body       | UPPER         | CharacterCustomizeItem | Character/Item/Customize  |
| 10       | Full Body        | FULL_BODY     | CharacterCustomizeItem | Character/Item/Customize  |
| 11       | Lower Body       | LOWER         | CharacterCustomizeItem | Character/Item/Customize  |
| 12       | Upper Accessory  | UPPER_ACC     | CharacterCustomizeItem | Character/Item/Customize  |
| 13       | Lower Accessory  | LOWER_ACC     | CharacterCustomizeItem | Character/Item/Customize  |
| 14       | Extra            | EXTRA         | CharacterCustomizeItem | Character/Item/Customize  |
| 15       | Effects          | -             | EffectCharacterItem    | Character/Item/EffectItem |
| 17       | Aura             | -             | AuraCharacterItem      | Character/Item/AuraItem   |

**G**: Unknown (int16)

**H**: Name by ID

**I**: Name of item
Example:
The value `NSD_CUS_007` will point the Lidia's swimsuit's item name

**J**: Unknown
Usually 0

**K**: Slot Group ID

| Slot Group IDs | Slot Group Description |
| -------------- | ---------------------- |
| -236           | Mirroring              |
| -237           | Skin Tone              |
| -238           | Aura                   |
| -240           | Effects                |
| -242           | Lower Accessory        |
| -243           | Upper Accessory        |
| -244           | Lower Body             |
| -245           | Full Body              |
| -246           | Upper Body             |
| -247           | Face Paint             |
| -249           | Face                   |
| -250           | Glasses                |
| -251           | Hair Accessory         |
| -252           | Full Head              |
| -253           | Hairstyle              |
| -254           | Head                   |

**L**: Unknown (int32)

**M**: PlayerCus ID
1 = 1p
2 = 2p
0 = Generic

**N**: Slot colors (0-255)
The amount of slot colors for this item, the max is 5. Any value above 5 will cause issues.
0 - Not colorable
1 - 1 Slot
2 - 2 slots
etc...

**O**: Unknown (0-255)
Usually 0

**P**: Rarity (0-5)
Used for the stars

**Q**: packageId (int32)
Used to indicate if the item is DLC, locked, unlocked, seems to be a slot flag

| Package IDs | Package Description                      |
| ----------- | ---------------------------------------- |
| -255        | Base                                     |
| -254        | PS4 Exclusive                            |
| -253        | Purchasable (Value after is cost)        |
| -243        | Treasure Battle (★★★★ Unique Tier Item)  |
| -242        | Treasure Battle (★★★★★ Unique Tier Item) |
| -237        | Treasure Battle (Heist Battle)           |
| -236        | Treasure Battle (★ Tier Shared Item)     |
| -235        | Treasure Battle (★★ Tier Shared Item)    |
| -234        | Treasure Battle (★★★ Tier Shared Item)   |
| -233        | Treasure Battle (★★★★ Shared Item)       |
| -232        | Treasure Battle (★★★★★ Shared Item)      |
| -218        | Ultimate Tekken Bowl DLC (First Unlock)  |
| -217        | Ultimate Tekken Bowl DLC (Second Unlock) |
| -212        | Season Pass 1                            |
| -211        | DLC Pack 1                               |
| -207        | Noctis DLC                               |
| -1          | Permalocked                              |

**R**: Item cost (int32)
The cost of the item

**S**: Unknown (int32)

**T**: Additional details about the item.
Example:
The value `item_text_008` will point to the textual description for Lucky Chloe's tail item that grows when she ranks up

**U**: Unknown

## See Also

- [TKDataPatcher Mod Page on TekkenMods](https://tekkenmods.com/mod/2301)
- [TkDataPatcher Repository on GitHub](https://github.com/DennisStanistan/TKDataPatcher)
