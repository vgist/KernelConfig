Gentoo qemu with utm.app

#### GCC optimization

    file: /etc/portage/make.conf

    COMMON_FLAGS="-O2 -pipe"
    CFLAGS="${COMMON_FLAGS}"
    CXXFLAGS="${COMMON_FLAGS}"
    FCFLAGS="${COMMON_FLAGS}"
    FFLAGS="${COMMON_FLAGS}"
    LDFLAGS="-Wl,-O2 -Wl,--as-needed"
    MAKEOPTS="-j8"
    CHOST="aarch64-unknown-linux-gnu"

#### USE flags

    file: /etc/portage/make.conf

    INPUT_DEVICES="libinput"
    VIDEO_CARDS="virgl"

