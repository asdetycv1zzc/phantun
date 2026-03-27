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
