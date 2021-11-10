# BedrockData
Blobs of data generated from Minecraft: Bedrock Edition used by PocketMine-MP

## canonical_block_states.nbt
This file contains an ordered list of `TAG_Compound`s (in varint NBT format) representing the pre-agreed blockstates in MCPE.
The runtime ID of a state is the offset in the list that the state appears.
The contents of this file are extracted from the vanilla BDS using [`pmmp/mapping`](https://github.com/pmmp/mapping).

## block_id_map.json
This file contains a mapping of all block stringy IDs to legacy numeric IDs (which are still used internally, and still needed by third party developers for conversion and for items).

#### Note
Where a block's legacy ID is > 255, its item ID is `255 - legacyBlockId`. This means prismarine stairs = -2 and so on.

## r12_to_current_block_map.bin
This file contains a list of mappings from legacy pre-1.13 blockstates to states of the current version.
This data is obtained by plugging the legacy states into `BlockPalette` in the vanilla BDS using [`pmmp/mapping`](https://github.com/pmmp/mapping), and writing the resulting NBT state obtained.

#### Schema
The following structure is repeated until EOF. There is **no** length prefix, so you have to read to EOF to read all the mappings.
| type | description |
|------|-------------|
| unsigned varint32 | r12 block string ID length |
| byte[] | r12 block string ID |
| little-endian int16 | r12 block metadata |
| TAG_Compound (varint format) | current version NBT blockstate corresponding to the given r12 block |

An example of how to read this file using the PocketMine-MP core library can be seen on the [stable branch](https://github.com/pmmp/PocketMine-MP/blob/41f7c07703bf3f7ef2d9504bbdbdf74257e75d12/src/pocketmine/network/mcpe/convert/RuntimeBlockMapping.php#L71-L86) or on the [master branch](https://github.com/pmmp/PocketMine-MP/blob/73592349cd29d91b03c2703107db859115a92e2d/src/network/mcpe/convert/RuntimeBlockMapping.php#L70-L80).

## r16_to_current_block_map.json
This file contains mappings to translate pre-1.16.100 item IDs into post-1.16.100 item IDs.
It contains two structures:
- `simple`: these are 1:1 replacement IDs (e.g. `carrotonastick` was renamed to `carrot_on_a_stick`)
- `complex`: these are items that were previously metadata values on other items (e.g. `dye:4` is now represented by `lapis_lazuli`).

## item_id_map.json
This file contains a mapping of all item stringy IDs to legacy numeric IDs.

## banner_patterns.json
This file defines all the known banner pattern types and their crafting requirements.

## recipes.json
This file defines all crafting-table, furnace and chemistry recipes. This includes recipes for the smoker, cartography table etc.

## creativeitems.json
This file contains an ordered list of items which appear in the vanilla creative inventory with Education Edition and Experimental Gameplay enabled.

## biome_definitions.nbt
This file contains a network-format NBT blob containing biome definitions obtained from `BiomeDefinitionListPacket`.

## biome_id_map.json
This file contains a mapping of Minecraft string biome IDs to their legacy integer ID counterparts. While biome IDs aren't dynamic yet, it's expected they will become dynamic in the future.

## entity_identifiers.nbt
This file contains a network-format NBT blob containing entity identifier mappings obtained from `AvailableActorIdentifiersPacket`.
