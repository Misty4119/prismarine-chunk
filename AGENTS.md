# Agent instructions for prismarine-chunk

## Read first

Read [CONTEXT.md](CONTEXT.md) before changing chunk formats, palettes, lighting, serialization, or version dispatch. Read [SECURITY.md](SECURITY.md) before handling untrusted chunk bytes, NBT, fixtures from servers, or potentially large allocations. Use [README.md](README.md) for the public API and [types/index.d.ts](types/index.d.ts) for the TypeScript contract.

## Source of truth

- index.js re-exports src/index.js; src/index.js dispatches by registry.type and registry.version.majorVersion.
- src/pc/ contains Java Edition implementations and src/bedrock/ contains Bedrock implementations. Their wire formats are not interchangeable.
- src/pc/common/ and src/bedrock/common/ hold shared section, palette, light, blob, and utility code.
- test/ contains version fixtures and behavior tests. Binary fixtures are protocol evidence; do not normalize them casually.
- types/ is part of the public API. Update declarations when public JavaScript behavior changes.

## Change rules

- Preserve coordinate bounds, negative-Y section indexing, palette/global-state semantics, light masks, biome layouts, block entities, and serialization round trips.
- Add a version dispatch entry only when the implementation really matches that registry version. Do not alias a version to an older implementation to hide an unsupported wire change.
- Keep Java PC and Bedrock behavior separate. A passing PC fixture does not validate a Bedrock subchunk.
- Avoid unbounded allocation when reading lengths, palettes, block entities, or NBT from a network source. See SECURITY.md.
- Keep JSON helpers and binary dumps deterministic where existing tests depend on them.

## Commands and completion

From the repository root:

    npm install
    npm run lint
    npm test

npm test runs lint first. Use a focused version fixture while iterating, then the complete suite. Before handoff check TypeScript declarations, fixtures, and git diff --check. Record the version and wire evidence for a new implementation.


## Reproducing this fork's release work

The Java 26.2/26.3 dispatch and fixtures are newer than the current published package. npm install in an isolated clone does not reproduce the sibling data/protocol commits used by the fork. Use the adjacent repositories and exact commits documented by the consuming Mineflayer checkout when validating cross-repository behavior.
