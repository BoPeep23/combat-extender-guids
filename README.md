# Combat Extender Guids

### Features:
- Contains a hashmap (guid_mapper_input.json) with every guid (creature id) that I've found to date
- Generates Override json blocks for CombatExtender.json
- Generates Clone json blocks for CombatExtender.json

## guid_mapper_input.json
This is a JSON file containing a hashmap of **guid**. Before modifying this file, read
[Zehtukae's Clone Guide](https://github.com/Zehtukae/combat-extender/blob/main/clone-guide.md), which
explains where the guids come from and what they're used for.

```json
[
    {
        "Location": "Nautiloid",
        "Entity": "Entity (0200000100004658)",
        "Guid": "5c5bd50b-147e-42e5-93cb-59a754df91ec",
        "Handle": "Imp",
        "Name": "S_FirstFight_Imp_Melee_02",
        "Type": "",
        "HealthOverride": 10,
        "CloneTemplateGuid": "",
        "CloneDisplayName": "",
        "SpellsToAdd": [
            "Target_Sting_Imp"
        ],
        "PassivesToAdd": [],
        "Notes": []
    },
    {
        "Location": "Nautiloid",
        "Entity": "Entity (0200000100004648)",
        "Guid": "07f1512b-efe9-4eb8-8ade-783435e9d7c9",
        "Handle": "Imp",
        "Name": "S_FirstFight_Imp_Melee_01",
        "Type": "",
        "HealthOverride": 0,
        "CloneTemplateGuid": "",
        "CloneDisplayName": "Cloned Imp",
        "SpellsToAdd": [],
        "PassivesToAdd": [
            "ElementalAdept_Fire",
            "MAG_LC_BurnOnDamage_Scimitar_Passive"
        ],
        "Notes": []
    },
    {
        "Location": "Nautiloid",
        "Entity": "Entity (0200000100004650)",
        "Guid": "38d1ce2f-4f19-4266-971d-61f08bc1a895",
        "Handle": "Imp",
        "Name": "S_FirstFight_Imp_Ranged_01",
        "Type": "",
        "HealthOverride": 0,
        "CloneTemplateGuid": "S_FirstFight_Imp_Melee_02_5c5bd50b-147e-42e5-93cb-59a754df91ec",
        "CloneDisplayName": "Cloned Melee Imp",
        "SpellsToAdd": [],
        "PassivesToAdd": [],
        "Notes": []
    }
]
```

#### Location
This is a manually-added key-value pair that details the general location of the guid. Has no practica l
use in cloning or overriding, but is kept here for reference and searching.

#### Entity
Unique, generated field when searching for guids in the Script Extender Debug Console. Has no practical
use in cloning or overriding, but is kept here for reference.

#### Guid
Unique, generated field when searching for guids in the Script Extender Debug Console. This is one of two
values that are needed to generate template ids.

#### Handle
Generated field when searching for guids in the Script Extender Console. In-game, the value of Handle
is a creature's display name.

#### Name
Uniquely generated field when searching for guids in the Script Extender Console. This is one of two values
that are needed to generate template ids.

#### Type
This is a manually-added field that can be used for categorization purposes. I have some pre-written values
in these, but can be adjusted for organization. Has no bearing on the generation of Overrides and Clones.

#### HealthOverride
This is a manually-added field that is defaulted to 0. When you set this to another value, it will generate
an attribute in the output file guid_mapper_output_overrides.json that you can then paste into your
[CombatExtender.json](https://github.com/Zehtukae/combat-extender/blob/main/Source/CombatExtender.json)
"Overrides" section.

#### CloneTemplateGuid
Need to populate this field if you want to generate a Clone and that cloned enemy is DIFFERENT than the reference
enemy. 

For example, if you want to add a Magma Elemental near Lorroakan, instead of just cloning Lorroakan),
then you can populate the CloneTemplateGuid field under Lorroakan's section with the format "{Name}_{Guid}".
In this example, Lorroakan's block in guid_mapper_input.json would look as follows:

```json
[
    {
        "Location": "Ramazith's Tower",
        "Entity": "Entity (02000011000231f9)",
        "Guid": "a9d4b71d-b0ef-429e-8210-6dc8be986ee9",
        "Handle": "Lorroakan",
        "Name": "S_LOW_Lorroakan",
        "Type": "Human",
        "HealthOverride": 0,
        "CloneTemplateGuid": "S_LOW_LavaElemental_1dea794e-2657-4c54-916b-edc639c4ff8c",
        "CloneDisplayName": "Cloned Lava Elemental",
        "SpellsToAdd": [],
        "PassivesToAdd": [],
        "Notes": []
    }
]
```
***Leave this field empty if you do not fit the use case above.***
***If you add a value to CloneTemplateGuid, you should also add a CloneDisplayName value.***

#### CloneDisplayName
Need to populate this field if you want to generate a Clone. This is basically the UI display name of
your cloned enemy, regardless of whether you have a CloneTemplateGuid value.

If this field is not populated, the cloned enemy will appear as "No Display Name Found" in-game.

#### SpellsToAdd
A list of [Spells](https://bg3.norbyte.dev/search?q=type%3Aspell) that you want to add as an Override
to this guid.

#### PassivesToAdd
A list of [Passives](https://bg3.norbyte.dev/search?q=type%3Apassive) that you want to add as an
Override to this guid.

#### Notes
Manually added field. I've tried to include a few notes on these but they are far from comprehensive.
Has no effect on the generated json file.

## Generating files

### Step 1: Configuration to generate Overrides
In your guid_mapper_input_json, you'll need to modify at least one of these three fields on a creature's
json block to successfully output an Override:
* HealthOverride
* PassivesToAdd
* SpellsToAdd

Once you have configured your guid_mapper_input.json, run format_guid.exe (make sure it is in the same
folder as your guid_mapper_input.json). After this finishes running, a new file in the same folder
will be created called "guid_mapper_output_overrides.json". You can now copy the contents of this
outputted file and paste it in your "Overrides" section of the CombatExtender.json.

You can ignore or delete the "dummy_template" from the end of the json - it won't affect anything
regardless, it's mostly just there as a placeholder at the end of the json map.

### Step 2: Configuration to generate a Clone of the same enemy
In your guid_mapper_input_json, you'll need to modify the following field on a creature's json
block to successfully output the Clone:
* CloneDisplayName

***Leave the CloneTemplateGuid empty***.

### Step 3: Configuration to generate a Clone, but use a different enemy as a template
In your guid_mapper_input_json, you'll need to modify the following fields on a creature's json
block to successfully output the Clone:
* CloneDisplayName
* CloneTemplateGuid

### Step 4: Run format_guid.exe

Once you have configured your guid_mapper_input.json, run format_guid.exe (make sure it is in the same
folder as your guid_mapper_input.json). After this finishes running, one or two new file in the same folder
will be created called:
* guid_mapper_output_overrides.json
* guid_mapper_output_clones.json

You can now copy the contents of these files and paste them to your "Overrides" and "Clones" sections
(respectively) of the CombatExtender.json.

You can ignore or delete the "dummy_template" from the end of the json - it won't affect anything
regardless, it's mostly just there as a placeholder at the end of the json map.
