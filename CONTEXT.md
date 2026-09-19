# prismarine-chunk repository context

## Purpose

prismarine-chunk provides version-selected Java and Bedrock chunk-column and section classes. It is used by Mineflayer and other PrismarineJS modules to read, mutate, serialize, and inspect world data. The package is version 1.41.0 in the current tree and declares Node.js 14 or newer.

## Entry point and dispatch

index.js exports src/index.js. The loader accepts a version string or a prismarine-registry object, verifies registry.version, and dispatches on registry.version.type and registry.version.majorVersion. Java 26.1, 26.2, and 26.3 currently use the 1.18+ PC implementation in src/pc/1.18/chunk; that mapping is valid only because the tested wire and section layout is compatible. Bedrock uses separate implementations for 0.14, 1.0, 1.3/1.16/1.17, and 1.18/1.19/1.20/1.21 families.

## Data model

Common APIs cover block lookup and mutation, block state IDs, block entities, JSON conversion, and initialization. PC classes add section masks, palettes, biomes, block light, sky light, and network dump/load. Bedrock classes add subchunk encoding, blob/cache handling, runtime/network persistence storage, entities, and block entities. Coordinates may include negative Y on modern PC worlds; section indexing must adjust for the configured minY.

The public TypeScript contract is in types/index.d.ts. Keep it aligned with JavaScript exports, including optional Bedrock methods and the section/subchunk types.

## Fixtures and validation

test/ contains version directories and raw or generated protocol fixtures. PC fixtures validate palette packing, global state IDs, section masks, light arrays, biome serialization, height-related behavior, and round trips. Bedrock fixtures validate level chunks, subchunks, blobs, cache responses, and persistence variants. A fixture is part of the evidence for the wire format; document its source and avoid replacing it with a synthetic case when a real capture is available.

The package scripts are npm test (Mocha with lint through pretest), npm run lint, and npm run fix. The package does not publish a separate build step.

## Dependency relationships

prismarine-registry selects the version and edition; prismarine-block and prismarine-biome provide semantic objects; prismarine-nbt handles block-entity data; vec3 supplies coordinates; smart-buffer, uint4, and xxhash-wasm support binary storage and hashing. Mineflayer consumes the resulting chunk API and the sibling protocol/data packages.

## Version compatibility

Java 26.2 and 26.3 are exposed by dispatch entries and share the modern PC implementation. This is not evidence that every future protocol change can share it. Check minecraft-data and node-minecraft-protocol before adding or changing an entry, and add a fixture or regression test for each changed wire behavior.

## Common pitfalls

- Using a world coordinate as a section index without applying minY.
- Treating a palette index as a global state ID.
- Reading sky light in a Bedrock fixture with PC assumptions.
- Mutating a shared palette or section without preserving compactness and bit width.
- Loading a malicious length or NBT payload without a bound.
- Updating JavaScript methods without updating types/index.d.ts.


## Published versus local source

Java 26.2 and 26.3 support in this tree is after the current published 1.41.0 package. An isolated npm install is not sufficient to reproduce that source. Cross-repository validation uses adjacent PrismarineJS checkouts with local file dependencies; capture the exact dependency commits in reports.
