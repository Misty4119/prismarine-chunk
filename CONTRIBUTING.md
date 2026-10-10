# Contributing to prismarine-chunk

`src/index.js` selects a Java or Bedrock chunk implementation from the registry edition and major version. Preserve existing version dispatch when changing shared code. Java 26.3 (protocol 777) uses the 1.18+ section layout, including the 26.1 fluid count, and reads byte-array light masks. Bedrock subchunks have a separate wire format.

## Data and protocol provenance

The 26.3 registry and protocol schema come from the merged [`minecraft-data` source](https://github.com/Misty4119/minecraft-data/commit/1d37505b338710f2f347aa00efaa48a054754b19). The corresponding [`node-minecraft-protocol` merge](https://github.com/Misty4119/node-minecraft-protocol/commit/cc1bfca769d36e90f8941138c1f63144e55d3403) handles protocol 777 packet decoding. This package does not directly depend on `node-minecraft-protocol`; its CI generates an exact `minecraft-data` runtime and checks the chunk implementation against that runtime. Record new merged SHAs when either source changes.

## Checks

Run from the repository root:

```sh
npm install
npm run lint
npx mocha test/ChunkColumn.test.js test/pc.test.js test/test.js --grep '26\.3' --exit
npm test
git diff --check
```

`npm test` runs lint through `pretest`. A local npm install alone may use published data that differs from the sibling source tree; use the CI data checkout and prepare steps to reproduce 26.3. Keep the older Java and Bedrock tests in the full suite. Add a focused fixture or round-trip check when a chunk or light wire field changes. Update `types/index.d.ts` when a public API changes.

Do not commit live server addresses, private world data, credentials, or raw captures with identifying details. Follow [SECURITY.md](SECURITY.md) for malformed input and vulnerability handling.
