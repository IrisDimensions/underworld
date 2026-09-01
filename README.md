# Underworld

Underworld is an Iris pack that preserves the shipping Overworld pack's terrain graph at the same seed and coordinates, then replaces its materials, biomes, caves, deposits, vegetation, objects, loot, ambience, and ecology with Nether counterparts.

## Generation contract

- `underworld` is the active dimension. It keeps the Overworld height, continents, regions, biome selection, generators, caves, and coordinate inputs, while using `environment: NETHER` and `coordinateScale: 1.0`.
- `upperDimension` is intentionally empty, and the pack contains no roof dimension or upper-dimension resources.
- The active dimension uses full ambient block lighting and exact vanilla-Nether fog colors for each biome derivative. Its oceans, surface fluid, and enabled cave aquifers all resolve through the dimension fluid palette to lava; no lower pack palette places water or a waterlogged block.
- Terrain-first hydrology uses independently budgeted surface, underground, and deep-lava sources in 1,024-block planning tiles. Surface density is `1.75` with 384-block source spacing; underground density is `1.5` with 640-block spacing; deep lava density is `0.5` with 1,024-block spacing. Surface courses must start exposed, contain at least 384 blocks of exposed channel, and retain a continuous exposed reach; complete underground courses shorter than 384 blocks are discarded. Each tile publishes at most one outlet network with one complete surface main stem, avoiding manufactured tributary fans. When the initial inland outlet yields no valid surface course, a bounded ranked fallback tries alternate legal grottos without weakening terrain admission. Complete courses use 192-block primary meanders with restrained 48-block detail and whole-course terrain-aware smoothing. Lava channels are five to ten blocks wide and one to three blocks deep, with a fixed two-block centerline inset, tapered spring headwaters, a broad thalweg, and 12-to-24-block dry terrain blending into organic banks. Every horizontal wet cell and cascade head is recessed below its exact natural terrain and all four cardinal neighbors; unsupported styled edges are omitted instead of becoming spill paths. Nearby elevation losses collapse into one transition and one compact receiver; exposed gradients use one-block steps, proven falls use fluid-only curtains, and cuts beyond the strict six-block open-channel limit enter a contained mini-grotto through rounded full-width portals before reopening. Policy multipliers may tighten that limit but cannot deepen it. Underground routes connect to existing caves when containment succeeds, inland grottos provide 10 blocks of dry headroom, and deep lava remains in isolated pools without channel offshoots.
- Lower terrain, cave, decorator, procedural, and object palettes use Nether terrain, vegetation, lighting, ores, and native Nether-structure materials instead of Overworld blocks.
- Cave materials and reachable cave objects contain no dirt or grass blocks. The former glowstone-dominant cave family now uses wavy Simplex contour stripes of obsidian and crying obsidian with glowstone limited to a sparse stripe accent; standalone glowstone surface and ceiling decorators were removed from those caves.
- Magnetics selects between the same narrow vascular Magnetic Hollows, warped Flux Crystal Caverns, and Polarity Grotto geometry as Overworld. Magnetic Hollows uses connected galleries with occasional cellular polarity vaults instead of broad merged rooms; crying obsidian, glowstone, and Nether-converted crystal or monolith assets retain the geometry with Nether-safe materials. Its nine floating-biome entries also share Overworld's variable vascular or crystalline tails, coherent edge-taper variation, and restrained wall warp instead of fixed-depth slab undersides.
- Every active surface biome exposes sparse Nether quartz through a coherent Simplex gate calibrated to roughly 2% of surface columns, or about 5.1 candidates per chunk before competition with the biome's other decorators. Accepted columns form irregular connected surface veins averaging roughly 5–8 blocks per cluster instead of independent one-block flecks. Underground quartz and Nether gold use Minecraft 26.2's 16/10 attempts, configured vein sizes, netherrack-only host rule, and band from 10 blocks above the bottom to 10 below the top. Ancient debris uses the vanilla large absolute-Y 8..24 triangular pass and small bottom-to-top pass, both with sparse candidate shapes, full air-exposure rejection, and Nether-base-stone hosts. Regional duplicate ore and gilded-blackstone deposits are removed.
- All lower biomes have unique custom biome IDs, exact derivative fog colors, Nether ambience, no precipitation, and one of the five vanilla Nether derivatives. Native derivative spawn tables remain the baseline ecology. A compact Iris ambient layer supplements them with derivative-themed Piglins, Zombified Piglins, Hoglins, Magma Cubes, Skeletons, Endermen, and lava-surface Striders; aerial Ghasts remain native-only so their vanilla placement rules are preserved. Global and per-chunk cooldowns keep the supplemental population bounded, and every spawn entry uses equal rarity for Bukkit/modded parity. Arbitrary fire and soul-fire surface decorators run at 30% of their former fire-placement rate without reducing the non-fire entries in mixed palettes.
- Fortress, Bastion Remnant, Nether Fossil, and Nether Ruined Portal generation remains native, preserving registered content, weights, biome eligibility, start logic, processors, entities, spawners, loot, structure-specific mobs, and locate behavior. The `1.1` structure-set overrides reduce Nether Complex spacing from 27 to 26 (about 8% more candidates) and Ruined Portal spacing from 40 to 38 (about 11% more); Nether Fossils remain at spacing 2 because legal integer spacing cannot represent a 10% increase. Non-Nether vanilla structure families are denied as a second guard against Overworld template blocks, and `disabledExact` denies only `minecraft:ruined_portal` while leaving `minecraft:ruined_portal_nether` native.
- The copied editable structure graph, external datapacks, Overworld mob roster, orphaned assets, and unreachable resources are intentionally omitted. The replacement entity and spawner graph is Nether-only.

The lower terrain layout is designed to be coordinate-for-coordinate compatible with the source Overworld graph when both worlds use the same seed. Palette changes alter blocks but not the lower generator links, noise fields, biome lists, height ranges, or coordinate transforms.

## Install and validate

Current Iris builds do not download packs during startup. `/iris download pack=underworld` installs the flat-root stable asset at `https://github.com/IrisDimensions/underworld/releases/download/1005/underworld.zip`. Manual installation remains supported by extracting or copying this entire tree as `underworld` under the Iris packs root:

- Bukkit/Paper/Folia: `plugins/Iris/packs/underworld/`
- Fabric/Forge/NeoForge: `config/irisworldgen/packs/underworld/`

On Bukkit-family servers, validate with:

```text
/iris pack validate pack=underworld
/iris pack status pack=underworld
/iris pack package dimension=underworld obfuscate=false minify=true
```

Use Java 25 from a current Iris checkout for the same offline generation and bounded hydrology coverage gates used by publication:

```text
./gradlew --no-daemon :probe:genProbe \
  -PprobePack=/absolute/path/to/underworld \
  -PprobeDimension=underworld \
  -PprobeWarmupChunks=64 \
  -PprobeMeasuredChunks=256 \
  -PprobeStudio=true

./gradlew --no-daemon :probe:hydrologyPackProbe \
  -PprobePack=/absolute/path/to/underworld \
  -PprobeDimension=underworld \
  -PprobeSeeds=1,19,331,1337 \
  -PprobeMinimumTileX=8 \
  -PprobeMaximumTileX=23 \
  -PprobeMinimumTileZ=8 \
  -PprobeMaximumTileZ=23 \
  -PprobeRequiredCoverage=SURFACE_POOL@lava,RIFFLE@lava,CASCADE@lava,WATERFALL@lava,RIDGE_BORE@lava,UNDERGROUND_POOL@lava,UNDERGROUND_DROP@lava,SINKHOLE@lava,INLAND_GROTTO@lava,DEEP_POOL@deep_lava \
  -PprobeStudio=true

./gradlew --no-daemon :probe:generationOrderProbe \
  -PprobePack=/absolute/path/to/underworld \
  -PprobeDimension=underworld \
  -PprobeSeed=77 \
  -PprobeMinimumChunkX=2048 \
  -PprobeMaximumChunkX=2051 \
  -PprobeMinimumChunkZ=2048 \
  -PprobeMaximumChunkZ=2051 \
  -PprobeParallelism=4 \
  -PprobeShuffleSeed=1337 \
  -PprobeMulticore=false \
  -PprobeStudio=true
```

The generation probe runs the canonical pack validator before creating its test engine. The hydrology probe scans 1,024 explicit seed-tile combinations and fails unless every required feature/profile selector is accepted. The generation-order probe requires identical block and biome output across forward, reverse, shuffled, and bounded-parallel generation.

Create a disposable managed world with `/iris create underworld_test type=underworld seed=1337`. A managed `iris:*` world is not automatically the destination of vanilla Nether portals. To replace the selected save's actual Nether in place, back it up, run `/iris replace minecraft:the_nether type=underworld`, and restart once; Iris preserves the canonical Nether identity and seed while replacing its chunk store and generator. `coordinateScale: 1.0` supplies the 1:1 ratio.

The stable ZIP contains only the active lower-dimension resources.

## Pack publication

An unmarked commit at the head of `master` updates the mutable `beta` prerelease. If the full head commit message contains the literal, case-sensitive marker `V+`, beta publication is skipped and that exact commit is published as a stable release instead. The release tag is the positive integer `version` in `dimensions/underworld.json`, and the flat-root release asset is `underworld.zip`.

Stable version tags are immutable. Increment the dimension version before marking another commit with `V+`; publication fails if that version tag already belongs to a different commit. The `Publish V+ Pack Release` manual workflow defaults to a non-publishing dry run and also requires the selected commit to contain `V+`.

Both publication workflows build a flat-root `underworld.zip` from the exact commit and extract that candidate archive. Nothing is published unless Java 25 validation and the focused Studio generation, hydrology coverage, and generation-order gates all pass against that archive.

## Source and credits

Underworld continuously tracks the sibling IrisDimensions Overworld repository for terrain geometry while retaining independent ores and Nether-specific materials and runtime content. The original pack credits Astrash, ArMiN231, Brian, Coco, Cyberpwn, Espen, K530, RaydenKonig, RepixelatedMC, and Strangeone101. See `IrisDimensions-license.md` for the source license.
