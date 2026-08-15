# Underworld

Underworld is an Iris pack that preserves the shipping Overworld pack's terrain graph at the same seed and coordinates, then replaces its materials, biomes, caves, deposits, vegetation, objects, loot, ambience, and ecology with Nether counterparts.

## Generation contract

- `underworld` is the active dimension. It keeps the Overworld height, continents, regions, biome selection, generators, caves, and coordinate inputs, while using `environment: NETHER` and `coordinateScale: 1.0`.
- `upperDimension` is intentionally empty. The retained `underworld_roof` resources are dormant and do not generate until a future roof design is enabled deliberately.
- The active dimension uses full ambient block lighting and exact vanilla-Nether fog colors for each biome derivative. Its oceans, surface fluid, and enabled cave aquifers all resolve through the dimension fluid palette to lava; no lower pack palette places water or a waterlogged block.
- Lower terrain, cave, decorator, procedural, and object palettes use Nether terrain, vegetation, lighting, ores, and native Nether-structure materials instead of Overworld blocks.
- Cave materials and reachable cave objects contain no dirt or grass blocks. The former glowstone-dominant cave family now uses wavy Simplex contour stripes of obsidian and crying obsidian with glowstone limited to a sparse stripe accent; standalone glowstone surface and ceiling decorators were removed from those caves.
- Every active surface biome exposes sparse Nether quartz through a coherent Simplex gate calibrated to roughly 2% of surface columns, or about 5.1 candidates per chunk before competition with the biome's other decorators. Accepted columns form irregular connected surface veins averaging roughly 5–8 blocks per cluster instead of independent one-block flecks. Existing quartz deposits continue below the surface. Ancient debris uses a 75% chunk gate, one or two clumps of one or two blocks, with clump anchors at internal Y 5..51 (absolute world Y -251..-205).
- All lower biomes have unique custom biome IDs, exact derivative fog colors, Nether ambience, no precipitation, and one of the five vanilla Nether derivatives. Native derivative spawn tables remain the baseline ecology. A compact Iris ambient layer supplements them with derivative-themed Piglins, Zombified Piglins, Hoglins, Magma Cubes, Skeletons, Endermen, and lava-surface Striders; aerial Ghasts remain native-only so their vanilla placement rules are preserved. Global and per-chunk cooldowns keep the supplemental population bounded, and every spawn entry uses equal rarity for Bukkit/modded parity. Arbitrary fire and soul-fire surface decorators run at 30% of their former fire-placement rate without reducing the non-fire entries in mixed palettes.
- Fortress, Bastion Remnant, Nether Fossil, and Nether Ruined Portal generation remains native, preserving registered content, weights, biome eligibility, start logic, processors, entities, spawners, loot, structure-specific mobs, and locate behavior. The `1.1` structure-set overrides reduce Nether Complex spacing from 27 to 26 (about 8% more candidates) and Ruined Portal spacing from 40 to 38 (about 11% more); Nether Fossils remain at spacing 2 because legal integer spacing cannot represent a 10% increase. Non-Nether vanilla structure families are denied as a second guard against Overworld template blocks, and `disabledExact` denies only `minecraft:ruined_portal` while leaving `minecraft:ruined_portal_nether` native.
- The copied editable structure graph, external datapacks, Overworld mob roster, orphaned assets, and unreachable resources are intentionally omitted. The replacement entity and spawner graph is Nether-only and does not modify the dormant roof.

The lower terrain layout is designed to be coordinate-for-coordinate compatible with the source Overworld graph when both worlds use the same seed. Palette changes alter blocks but not the lower generator links, noise fields, biome lists, height ranges, or coordinate transforms.

## Install and validate

Current Iris builds install the Underworld beta release automatically at startup when `underworld` is absent. The release asset is a flat-root archive at `https://github.com/IrisDimensions/underworld/releases/download/beta/underworld.zip`; `/iris download underworld` uses the same asset. Manual installation remains supported by extracting or copying this entire tree as `underworld` under the Iris packs root:

- Bukkit/Paper/Folia: `plugins/Iris/packs/underworld/`
- Fabric/Forge/NeoForge: `config/irisworldgen/packs/underworld/`

On Bukkit-family servers, validate with:

```text
/iris pack validate pack=underworld
/iris pack status pack=underworld
```

Create a disposable managed world with `/iris create underworld_test type=underworld seed=1337`. A managed `iris:*` world is not automatically the destination of vanilla Nether portals. To replace the selected save's actual Nether in place, back it up, run `/iris replace minecraft:the_nether type=underworld`, and restart once; Iris preserves the canonical Nether identity and seed while replacing its chunk store and generator. `coordinateScale: 1.0` supplies the 1:1 ratio.

The beta ZIP retains `underworld_roof.json` and its resources, but `underworld` remains the selected dimension and its empty `upperDimension` keeps the roof dormant.

## Source and credits

Underworld is derived from the tracked IrisDimensions Overworld tree at commit `2d8fd0f9d90c6fc6ed095a6cedbbe5d9d0124556`. The original pack credits Astrash, ArMiN231, Brian, Coco, Cyberpwn, Espen, K530, RaydenKonig, RepixelatedMC, and Strangeone101. See `IrisDimensions-license.md` for the source license.
