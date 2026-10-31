# raknetify (Blockfield build)

Patched build of [Raknetify](https://github.com/RelativityMC/raknetify)
(MIT; the `-all` jars bundle
[netty-raknet](https://github.com/RelativityMC/netty-raknet), see
`LICENSE` and `modrinth_license.txt`).

## Provenance

- Upstream source: `RelativityMC/raknetify@76ab30a11a786daafb94c4ef95de457158cf80ce`
  (the commit behind published `0.1.0+alpha.5.196`), submodule
  `netty-raknet@15536296c2f478a512d291540a5791346cfe5fc2`.
- Patch: `patches/0001-release-sync-framedata.patch` — releases the SYNC
  `FrameData` in `SynchronizationLayer.channelRead` (Netty leak fix,
  tracked upstream).
- Build patch: `patches/0002-velocity-pinned-jar.patch` — the velocity
  module's `3.4.0-SNAPSHOT:540` ivy dependency resolved through
  api.papermc.io v2, which Paper sunset; the exact build is fetched from
  its content-addressed fill-API object URL with a pinned SHA-256
  (compile input only, never bundled into the plugin jar).
- Applied by CI at build time; the checked-in source tree is pristine
  upstream. CI builds `:common:build :fabric:build :velocity:build`
  (bungee skipped: dead snapshot host, jar unshipped).
- `master` here == upstream master at the pinned commit, plus this
  provenance (patch file, workflow, this file).

## Build

CI (`.github/workflows/build.yml`): applies `patches/*.patch`, runs
`./gradlew build`, then a bytecode check asserting the built
`SynchronizationLayer` calls `FrameData.release`. Tag `bfN` additionally
publishes `raknetify-{fabric,velocity}-*-bfN-all.jar` plus
`SHA512SUMS`/`SHA256SUMS` as a GitHub Release; the client pack and the
proxy pin those assets by URL + hash.
