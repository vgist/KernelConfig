Thinkpad X230

#### Wi-Fi

- sys-firmware/linux-firmware

#### Bluetooth

- sys-firmware/broadcom-bt-firmware

#### Microcode

- sys-firmware/intel-microcode

more: <https://wiki.gentoo.org/wiki/Intel_microcode>

#### GCC optimization

```
file: /etc/portage/make.conf

COMMON_FLAGS="-march=ivybridge -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j4"
```

#### CPU_FLAGS_X86

- app-portage/cpuid2cpuflags

```
file: /etc/portage/make.conf

CPU_FLAGS_X86="aes avx f16c mmx mmxext pclmul popcnt rdrand sse sse2 sse3 sse4_1 sse4_2 ssse3"
```

#### USE flags

```
file: /etc/portage/make.conf

INPUT_DEVICES="libinput"
VIDEO_CARDS="intel i965"
```

#### EFI stub kernel

- sys-boot/efibootmgr

Need kernel configuration

```
Processor type and features  --->
    [*] EFI runtime service support
    [*]   EFI stub support
    [ ]     EFI mixed-mode support
```

    mount /dev/sda1 /boot
    mkdir -p /boot/EFI/Gentoo
    cp arch/x86_64/boot/bzImage /boot/EFI/Gentoo/gentoo.efi

Boot with EFI stub kernel. You need efibootmgr to add boot option

    efibootmgr -c -d /dev/sda -p 1 -l "\EFI\Gentoo\gentoo.efi" -L "Gentoo Linux" -u "root=PARTUUID=xxxxxxxxxx ro init=/lib/systemd/systemd quiet i915.lvds_downclock=1 i915.semaphores=1 i915.fastboot=1"

Also rescue mode

    efibootmgr -c -d /dev/sda -p 1 -l "\EFI\Gentoo\gentoo.efi" -L "Gentoo Linux (rescue)" -u "root=PARTUUID=xxxxxxxxxx init=/bin/sh"

You can get partuuid with `blkid` command that belongs to `sys-apps/util-linux`

Get more information on [EFI stub kernel](https://wiki.gentoo.org/wiki/EFI_stub_kernel).

#### Hardware information

<details>
<summary>Details</summary>

```
root # lscpu
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   36 bits physical, 48 bits virtual
CPU(s):                          4
On-line CPU(s) list:             0-3
Thread(s) per core:              2
Core(s) per socket:              2
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       GenuineIntel
CPU family:                      6
Model:                           58
Model name:                      Intel(R) Core(TM) i7-3520M CPU @ 2.90GHz
Stepping:                        9
CPU MHz:                         3285.021
CPU max MHz:                     3600.0000
CPU min MHz:                     1200.0000
BogoMIPS:                        5787.03
Virtualization:                  VT-x
L1d cache:                       64 KiB
L1i cache:                       64 KiB
L2 cache:                        512 KiB
L3 cache:                        4 MiB
NUMA node0 CPU(s):               0-3
Vulnerability Itlb multihit:     KVM: Mitigation: VMX disabled
Vulnerability L1tf:              Mitigation; PTE Inversion; VMX conditional cache flushes, SMT vulnerable
Vulnerability Mds:               Mitigation; Clear CPU buffers; SMT vulnerable
Vulnerability Meltdown:          Mitigation; PTI
Vulnerability Spec store bypass: Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Full generic retpoline, IBPB conditional, IBRS_FW, STIBP conditional, RSB filling
Vulnerability Srbds:             Vulnerable: No microcode
Vulnerability Tsx async abort:   Not affected
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fx
                                 sr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl
                                 xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse
                                 3 cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand
                                 lahf_lm cpuid_fault epb pti ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid fsgsbase
                                  smep erms xsaveopt dtherm ida arat pln pts md_clear flush_l1d
```

```
root # lspci -nnk
00:00.0 Host bridge [0600]: Intel Corporation 3rd Gen Core processor DRAM Controller [8086:0154] (rev 09)
	Subsystem: Lenovo 3rd Gen Core processor DRAM Controller [17aa:21fa]
	Kernel driver in use: ivb_uncore
00:02.0 VGA compatible controller [0300]: Intel Corporation 3rd Gen Core processor Graphics Controller [8086:0166] (rev 09)
	Subsystem: Lenovo 3rd Gen Core processor Graphics Controller [17aa:21fa]
	Kernel driver in use: i915
00:14.0 USB controller [0c03]: Intel Corporation 7 Series/C210 Series Chipset Family USB xHCI Host Controller [8086:1e31] (rev 04)
	Subsystem: Lenovo 7 Series/C210 Series Chipset Family USB xHCI Host Controller [17aa:21fa]
	Kernel driver in use: xhci_hcd
	Kernel modules: xhci_pci
00:16.0 Communication controller [0780]: Intel Corporation 7 Series/C216 Chipset Family MEI Controller #1 [8086:1e3a] (rev 04)
	Subsystem: Lenovo 7 Series/C216 Chipset Family MEI Controller [17aa:21fa]
00:16.3 Serial controller [0700]: Intel Corporation 7 Series/C210 Series Chipset Family KT Controller [8086:1e3d] (rev 04)
	Subsystem: Lenovo 7 Series/C210 Series Chipset Family KT Controller [17aa:21fa]
00:19.0 Ethernet controller [0200]: Intel Corporation 82579LM Gigabit Network Connection (Lewisville) [8086:1502] (rev 04)
	Subsystem: Lenovo 82579LM Gigabit Network Connection (Lewisville) [17aa:21f3]
	Kernel driver in use: e1000e
00:1a.0 USB controller [0c03]: Intel Corporation 7 Series/C216 Chipset Family USB Enhanced Host Controller #2 [8086:1e2d] (rev 04)
	Subsystem: Lenovo 7 Series/C216 Chipset Family USB Enhanced Host Controller [17aa:21fa]
	Kernel driver in use: ehci-pci
	Kernel modules: ehci_pci
00:1b.0 Audio device [0403]: Intel Corporation 7 Series/C216 Chipset Family High Definition Audio Controller [8086:1e20] (rev 04)
	Subsystem: Lenovo 7 Series/C216 Chipset Family High Definition Audio Controller [17aa:21fa]
	Kernel driver in use: snd_hda_intel
00:1c.0 PCI bridge [0604]: Intel Corporation 7 Series/C216 Chipset Family PCI Express Root Port 1 [8086:1e10] (rev c4)
	Kernel driver in use: pcieport
00:1c.1 PCI bridge [0604]: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 2 [8086:1e12] (rev c4)
	Kernel driver in use: pcieport
00:1c.2 PCI bridge [0604]: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 3 [8086:1e14] (rev c4)
	Kernel driver in use: pcieport
00:1d.0 USB controller [0c03]: Intel Corporation 7 Series/C216 Chipset Family USB Enhanced Host Controller #1 [8086:1e26] (rev 04)
	Subsystem: Lenovo 7 Series/C216 Chipset Family USB Enhanced Host Controller [17aa:21fa]
	Kernel driver in use: ehci-pci
	Kernel modules: ehci_pci
00:1f.0 ISA bridge [0601]: Intel Corporation QM77 Express Chipset LPC Controller [8086:1e55] (rev 04)
	Subsystem: Lenovo QM77 Express Chipset LPC Controller [17aa:21fa]
	Kernel driver in use: lpc_ich
00:1f.2 SATA controller [0106]: Intel Corporation 7 Series Chipset Family 6-port SATA Controller [AHCI mode] [8086:1e03] (rev 04)
	Subsystem: Lenovo 7 Series Chipset Family 6-port SATA Controller [AHCI mode] [17aa:21fa]
	Kernel driver in use: ahci
00:1f.3 SMBus [0c05]: Intel Corporation 7 Series/C216 Chipset Family SMBus Controller [8086:1e22] (rev 04)
	Subsystem: Lenovo 7 Series/C216 Chipset Family SMBus Controller [17aa:21fa]
	Kernel driver in use: i801_smbus
02:00.0 System peripheral [0880]: Ricoh Co Ltd MMC/SD Host Controller [1180:e822] (rev 07)
	Subsystem: Lenovo MMC/SD Host Controller [17aa:21fa]
	Kernel driver in use: sdhci-pci
	Kernel modules: sdhci_pci
03:00.0 Network controller [0280]: Intel Corporation Centrino Advanced-N 6205 [Taylor Peak] [8086:0085] (rev 34)
	Subsystem: Intel Corporation Centrino Advanced-N 6205 (802.11a/b/g/n) [8086:1311]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
```

```
root # lsusb
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 006: ID 04f2:b2eb Chicony Electronics Co., Ltd Integrated Camera
Bus 001 Device 005: ID 0a5c:21e6 Broadcom Corp. BCM20702 Bluetooth 4.0 [ThinkPad]
Bus 001 Device 004: ID 147e:2020 Upek TouchChip Fingerprint Coprocessor (WBF advanced mode)
Bus 001 Device 003: ID 046d:c52b Logitech, Inc. Unifying Receiver
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

```
root # dmidecode -t memory | grep -i size
	Size: 8 GB
	Size: 8 GB
root # dmidecode -t memory | grep 'Memory Speed'
	Configured Memory Speed: 1600 MT/s
	Configured Memory Speed: 1600 MT/s
```

</details>
