# Underworld

Underworld is a two-dimension Iris pack that preserves the shipping Overworld pack's terrain graph at the same seed and coordinates, then replaces its materials, biomes, caves, deposits, vegetation, objects, loot, ambience, and ecology with Nether counterparts.

## Generation contract

- `underworld` is the active dimension. It keeps the Overworld height, continents, regions, biome selection, generators, caves, and coordinate inputs, while using `environment: NETHER` and `coordinateScale: 1.0`.
- `underworld_roof` is the physical inverted ceiling profile referenced by `upperDimension`. It supplies five roof terrain themes and sparse upside-down surface objects; it is not a separately travelable Minecraft world.
- All lower biomes have unique custom biome IDs, Nether colors and ambience, no precipitation, and one of the five vanilla Nether derivatives. Native derivative spawn tables provide Nether mobs without copied Overworld spawners.
- Fortress, Bastion Remnant, Nether Fossil, and Nether Ruined Portal generation remains native so vanilla processors, entities, spawners, loot, spacing, and structure-specific mobs remain intact.
- The copied editable structure graph, external datapacks, Overworld spawners/entities, orphaned assets, and unreachable resources are intentionally omitted.

The lower terrain layout is designed to be coordinate-for-coordinate compatible with the source Overworld graph when both worlds use the same seed. Palette changes alter blocks but not the lower generator links, noise fields, biome lists, height ranges, or coordinate transforms.

## Install and validate

Copy this entire folder to the Iris packs root as `underworld`:

- Bukkit/Paper/Folia: `plugins/Iris/packs/underworld/`
- Fabric/Forge/NeoForge: `config/irisworldgen/packs/underworld/`

On Bukkit-family servers, validate with:

```text
/iris pack validate pack=underworld
/iris pack status pack=underworld
```

Create a disposable managed world with `/iris create underworld_test type=underworld seed=1337`. A managed Iris world is not automatically the destination of vanilla Nether portals. To make this pack the server's actual Nether, route portals to it or assign the real `<level-name>_nether` world to Iris on a backed-up, disposable test server before regenerating production data. `coordinateScale: 1.0` supplies the 1:1 ratio; portal destination selection is server/platform configuration.

Distribute and install the raw folder. Current Iris download/export tooling expects a single dimension and does not include a distinct `upperDimension` closure in a normal `.iris` package.

## Source and credits

Underworld is derived from the tracked IrisDimensions Overworld tree at commit `2d8fd0f9d90c6fc6ed095a6cedbbe5d9d0124556`. The original pack credits Astrash, ArMiN231, Brian, Coco, Cyberpwn, Espen, K530, RaydenKonig, RepixelatedMC, and Strangeone101. See `IrisDimensions-license.md` for the source license.
