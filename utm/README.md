Gentoo qemu with utm.app

#### GCC optimization

    file: /etc/portage/make.conf

    COMMON_FLAGS="-O3 -pipe"
    CFLAGS="${COMMON_FLAGS}"
    CXXFLAGS="${COMMON_FLAGS}"
    FCFLAGS="${COMMON_FLAGS}"
    FFLAGS="${COMMON_FLAGS}"
    LDFLAGS="-Wl,-O2 -Wl,--as-needed"
    MAKEOPTS="-j8"
    CHOST="aarch64-unknown-linux-gnu"

#### CPU_FLAGS_ARM

- app-portage/cpuid2cpuflags

```
    file: /etc/portage/make.conf

    CPU_FLAGS_ARM="edsp neon thumb vfp vfpv3 vfpv4 vfp-d32 aes sha1 sha2 crc32 v4 v5 v6 v7 v8 thumb2"
```

#### USE flags

    file: /etc/portage/make.conf

    INPUT_DEVICES="libinput"
    VIDEO_CARDS="virgl"

