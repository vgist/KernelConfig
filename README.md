#### Devices

  - Thinkpad X230
  - Thinkpad X280
  - utm.app on m1 MacBook Pro

#### Compressed kernel modules

Please note, kernel configuration files here used to contain compression of kernel and modules, so you should emerge `sys-apps/kmod` with below USE flags before compiling your kernel and modules

    file: /etc/portage/package.use

    sys-apps/kmod lzma zlib zstd

#### Kernel Configuration

Generate basic config file

    make savedefconfig

Restore from basic config file

    cd /usr/src/linux/
    cp /path/your_defconfig .config
    make olddefconfig

or

    cd /usr/src/linux/
    cp /path/your_defconfig arch/your-srcarch/configs/your_new_defconfig
    make your_new_defconfig

Build binary

    make -j`nproc` && make modules_install

Read more: <https://wiki.gentoo.org/wiki/Kernel/Configuration>
