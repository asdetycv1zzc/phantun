# Phantun OpenWrt Feed Package

This repository includes a ready-to-use OpenWrt package at:

- `net/phantun`

## Use as custom feed

1. Put this directory somewhere reachable by OpenWrt, for example:

   `src-link phantun /path/to/phantun/openwrt-feed`

   in your OpenWrt `feeds.conf` or `feeds.conf.default`.

2. Update and install feed packages:

   ```sh
   ./scripts/feeds update phantun
   ./scripts/feeds install -a -p phantun
   ```

3. Select and build:

   ```sh
   make menuconfig
   # Network -> VPN -> phantun
   make package/phantun/compile V=s
   ```

## Runtime config

After installation, edit `/etc/config/phantun` and enable one or more `config instance` sections.
Service entrypoint is `/etc/init.d/phantun`.

## Fast feedback loop (avoid 20-minute failures)

Before full compile, run this quick path in SDK:

```sh
SDK=/root/openwrt-sdk-25.12.0-x86-64_gcc-14.3.0_musl.Linux-x86_64
cd "$SDK"

# 1) Only unpack/prepare package sources
make package/feeds/phantun/phantun/prepare -j"$(nproc)"

# 2) Validate Cargo manifest path (workspace vs real crate)
BUILD_DIR=$(ls -d "$SDK"/build_dir/target-*/phantun-0.8.1 | head -n1)
CARGO_HOME="$SDK/dl/cargo" "$SDK/staging_dir/host/bin/cargo" metadata \
  --manifest-path "$BUILD_DIR/phantun/Cargo.toml" --locked --format-version 1 >/dev/null
```

If the above passes, then run full build:

```sh
make package/feeds/phantun/phantun/compile -j"$(nproc)"
```
