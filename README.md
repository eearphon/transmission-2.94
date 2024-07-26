## Description
Transmission 2.94 build with mbedtls for OpenWrt

## Use
```shell
#Add to first line
sed -i '1i\src-git transmission https://github.com/eearphon/transmission-2.94.git' feeds.conf.default

./scripts/feeds uninstall -a
rm -rf tmp/

./scripts/feeds update -a
./scripts/feeds install -a

make defconfig
```

## Links

- [GitHub - coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)

- [GitHub - OpenWrt](https://github.com/openwrt/openwrt)
