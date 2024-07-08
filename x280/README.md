Thinkpad X280

#### Wi-Fi

- sys-firmware/linux-firmware

#### Microcode

- sys-firmware/intel-microcode

more: <https://wiki.gentoo.org/wiki/Intel_microcode>

#### GCC optimization

```
file: /etc/portage/make.conf

COMMON_FLAGS="-march=skylake -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j8"
```

#### CPU_FLAGS_X86

- app-portage/cpuid2cpuflags

```
file: /etc/portage/make.conf

CPU_FLAGS_X86="aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt sha sse sse2 sse3 sse4_1 sse4_2 sse4a ssse3"
```

#### USE flags

```
file: /etc/portage/make.conf

INPUT_DEVICES="libinput"
VIDEO_CARDS="intel"
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

    efibootmgr -c -d /dev/sda -p 1 -l "\EFI\Gentoo\gentoo.efi" -L "Gentoo Linux" -u "root=PARTUUID=xxxxxxxxxx ro init=/lib/systemd/systemd quiet i915.enable_guc=3"

Also rescue mode

    efibootmgr -c -d /dev/sda -p 1 -l "\EFI\Gentoo\gentoo.efi" -L "Gentoo Linux (rescue)" -u "root=PARTUUID=xxxxxxxxxx init=/bin/sh"

You can get partuuid with `blkid` command that belongs to `sys-apps/util-linux`

Get more information on [EFI stub kernel](https://wiki.gentoo.org/wiki/EFI_stub_kernel).

#### Hardware information

<details>
<summary>Details</summary>

```
root # lscpu
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         39 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  8
  On-line CPU(s) list:   0-7
Vendor ID:               GenuineIntel
  Model name:            Intel(R) Core(TM) i5-8350U CPU @ 1.70GHz
    CPU family:          6
    Model:               142
    Thread(s) per core:  2
    Core(s) per socket:  4
    Socket(s):           1
    Stepping:            10
    CPU(s) scaling MHz:  19%
    CPU max MHz:         3600.0000
    CPU min MHz:         400.0000
    BogoMIPS:            3799.90
    Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art 
                         arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4
                         _1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb invpcid_single pti ssbd ibrs ibpb stibp tpr_shado
                         w vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid mpx rdseed adx smap clflushopt intel_pt xsaveopt xsavec xgetbv1 xsaves dtherm
                          ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp md_clear flush_l1d arch_capabilities
Virtualization features:
  Virtualization:        VT-x
Caches (sum of all):
  L1d:                   128 KiB (4 instances)
  L1i:                   128 KiB (4 instances)
  L2:                    1 MiB (4 instances)
  L3:                    6 MiB (1 instance)
NUMA:
  NUMA node(s):          1
  NUMA node0 CPU(s):     0-7
Vulnerabilities:
  Gather data sampling:  Vulnerable: No microcode
  Itlb multihit:         KVM: Mitigation: VMX disabled
  L1tf:                  Mitigation; PTE Inversion; VMX conditional cache flushes, SMT vulnerable
  Mds:                   Mitigation; Clear CPU buffers; SMT vulnerable
  Meltdown:              Mitigation; PTI
  Mmio stale data:       Mitigation; Clear CPU buffers; SMT vulnerable
  Retbleed:              Mitigation; IBRS
  Spec rstack overflow:  Not affected
  Spec store bypass:     Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:            Mitigation; usercopy/swapgs barriers and __user pointer sanitization
  Spectre v2:            Mitigation; IBRS, IBPB conditional, STIBP conditional, RSB filling, PBRSB-eIBRS Not affected
  Srbds:                 Mitigation; Microcode
  Tsx async abort:       Mitigation; TSX disabled
```

```
root # lspci -nnk
00:00.0 Host bridge [0600]: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers [8086:5914] (rev 08)
	Subsystem: Lenovo Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers [17aa:2256]
	Kernel driver in use: skl_uncore
00:02.0 VGA compatible controller [0300]: Intel Corporation UHD Graphics 620 [8086:5917] (rev 07)
	Subsystem: Lenovo UHD Graphics 620 [17aa:2256]
	Kernel driver in use: i915
	Kernel modules: i915
00:04.0 Signal processing controller [1180]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem [8086:1903] (rev 08)
	Subsystem: Lenovo Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem [17aa:2256]
	Kernel driver in use: proc_thermal
	Kernel modules: processor_thermal_device_pci_legacy
00:08.0 System peripheral [0880]: Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model [8086:1911]
	Subsystem: Lenovo Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model [17aa:2256]
00:14.0 USB controller [0c03]: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller [8086:9d2f] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP USB 3.0 xHCI Controller [17aa:2256]
	Kernel driver in use: xhci_hcd
	Kernel modules: xhci_pci
00:14.2 Signal processing controller [1180]: Intel Corporation Sunrise Point-LP Thermal subsystem [8086:9d31] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP Thermal subsystem [17aa:2256]
	Kernel driver in use: intel_pch_thermal
	Kernel modules: intel_pch_thermal
00:16.0 Communication controller [0780]: Intel Corporation Sunrise Point-LP CSME HECI #1 [8086:9d3a] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP CSME HECI [17aa:2256]
00:16.3 Serial controller [0700]: Intel Corporation Sunrise Point-LP Active Management Technology - SOL [8086:9d3d] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP Active Management Technology - SOL [17aa:2256]
00:1c.0 PCI bridge [0604]: Intel Corporation Sunrise Point-LP PCI Express Root Port #1 [8086:9d10] (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port [17aa:2256]
	Kernel driver in use: pcieport
00:1c.2 PCI bridge [0604]: Intel Corporation Sunrise Point-LP PCI Express Root Port #3 [8086:9d12] (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port [17aa:2256]
	Kernel driver in use: pcieport
00:1c.4 PCI bridge [0604]: Intel Corporation Sunrise Point-LP PCI Express Root Port #5 [8086:9d14] (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port [17aa:2256]
	Kernel driver in use: pcieport
00:1f.0 ISA bridge [0601]: Intel Corporation Sunrise Point LPC/eSPI Controller [8086:9d4e] (rev 21)
	Subsystem: Lenovo Sunrise Point LPC Controller/eSPI Controller [17aa:2256]
00:1f.2 Memory controller [0580]: Intel Corporation Sunrise Point-LP PMC [8086:9d21] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP PMC [17aa:2256]
00:1f.3 Audio device [0403]: Intel Corporation Sunrise Point-LP HD Audio [8086:9d71] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP HD Audio [17aa:2256]
	Kernel driver in use: snd_hda_intel
	Kernel modules: snd_hda_intel
00:1f.4 SMBus [0c05]: Intel Corporation Sunrise Point-LP SMBus [8086:9d23] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP SMBus [17aa:2256]
	Kernel driver in use: i801_smbus
	Kernel modules: i2c_i801
00:1f.6 Ethernet controller [0200]: Intel Corporation Ethernet Connection (4) I219-LM [8086:15d7] (rev 21)
	Subsystem: Lenovo Ethernet Connection (4) I219-LM [17aa:2256]
	Kernel driver in use: e1000e
	Kernel modules: e1000e
02:00.0 PCI bridge [0604]: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] [8086:15c0] (rev 01)
	Subsystem: Device [2222:1111]
	Kernel driver in use: pcieport
03:00.0 PCI bridge [0604]: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] [8086:15c0] (rev 01)
	Subsystem: Device [2222:1111]
	Kernel driver in use: pcieport
03:01.0 PCI bridge [0604]: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] [8086:15c0] (rev 01)
	Subsystem: Device [2222:1111]
	Kernel driver in use: pcieport
03:02.0 PCI bridge [0604]: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) [Alpine Ridge LP 2016] [8086:15c0] (rev 01)
	Subsystem: Device [2222:1111]
	Kernel driver in use: pcieport
04:00.0 System peripheral [0880]: Intel Corporation JHL6240 Thunderbolt 3 NHI (Low Power) [Alpine Ridge LP 2016] [8086:15bf] (rev 01)
	Subsystem: Device [2222:1111]
3a:00.0 USB controller [0c03]: Intel Corporation JHL6240 Thunderbolt 3 USB 3.1 Controller (Low Power) [Alpine Ridge LP 2016] [8086:15c1] (rev 01)
	Subsystem: Device [2222:1111]
	Kernel driver in use: xhci_hcd
	Kernel modules: xhci_pci
3b:00.0 Network controller [0280]: Intel Corporation Wireless 8265 / 8275 [8086:24fd] (rev 78)
	Subsystem: Intel Corporation Dual Band Wireless-AC 8265 [Windstorm Peak] [8086:0010]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
3c:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 [144d:a808]
	Subsystem: Samsung Electronics Co Ltd SSD 970 EVO [144d:a801]
	Kernel driver in use: nvme
```

```
root # lsusb
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 002: ID 045e:0956 Microsoft Corp. Microsoft Surface USB-C to DisplayPort Adapter
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 0bda:0316 Realtek Semiconductor Corp. Card Reader
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 004: ID 04f2:b604 Chicony Electronics Co., Ltd Integrated Camera (1280x720@30)
Bus 001 Device 003: ID 8087:0a2b Intel Corp. Bluetooth wireless interface
Bus 001 Device 002: ID 0000:3825   USB OPTICAL MOUSE
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

```
root # dmidecode -t memory | grep -e 'Memory Speed' -e Size
	Size: 8 GB
	Configured Memory Speed: 2400 MT/s
	Size: 8 GB
	Configured Memory Speed: 2400 MT/s
```

</details>
