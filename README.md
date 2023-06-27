# Hackintosh-UEFI-MSI-B760-13900K

This repo contains configuration and bootloader files that fully support macOS 12.6.5 Monterey with on MSI B760 motherboard and 13900k processor.

<p align="center">
<img src='https://raw.githubusercontent.com/barisalby/gist/main/images/Hackintosh-UEFI-MSI-B760-13900K/2225087433168.png'>
</p>

# 1. Preparation

### 1.1 Environment:

 <table>
<tr>
<th align="center">
<img width="441" height="1">
<p> 
 Device
</p>
</th>
<th align="center">
<img width="441" height="1">
<p> 
 Working
</p>
</th>
</tr>
 <tr>
      <td>13th Gen Intel(R) Core(TM) i9-13900K</td>
      <td>✅ <strong>WITH O.C. MODE</strong></td>
    </tr>
    <tr>
      <td>G.Skill Trident Z5 RGB 32 GB (2x16) 6000 MHz DDR5</td>
      <td>✅ <strong>WITH XMP MODE</strong></td>
    </tr>
    <tr>
      <td>iCUE H150i ELITE LCD XT Display Liquid CPU Cooler</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>Intel(R) Wireless Bluetooth(R)</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>Intel(R) Wi-Fi 6E AX211 160MHz</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>RTL8125 2.5GbE Controller</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>High Definition Audio Controller</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>Intel(R) SMBus - 7A23</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>Ellesmare Sapphire RX 580 8GB</td>
      <td>✅</td>
    </tr>
    <tr>
      <td>Intel(R) Shared SRAM - 7A27</td>
      <td>❌</td>
    </tr>
    <tr>
      <td>GeForce RTX 4070 Ti SUPRIM X 12G</td>
      <td>❌ <strong>DISABLED WITH SSDT PATCH</strong></td>
    </tr>
    <tr>
      <td>Intel Raptor Lake-S GT1 [UHD Graphics 770]</td>
      <td>❌ <strong>DISABLED WITH SSDT PATCH</strong></td>
    </tr>
</table>

<p align="center">
<img src='https://raw.githubusercontent.com/barisalby/gist/main/images/Hackintosh-UEFI-MSI-B760-13900K/6379423821685.png'>
</p>

### 1.2 BIOS

# 2. ACPI (Advanced Configuration and Power Interface)

### 2.1 Introduction

Advanced Configuration and Power Interface (ACPI) specification was developed to establish industry common interfaces enabling robust operating system (OS)-directed motherboard device configuration and power management of both devices and entire systems.

The pseudo-code language, known as ACPI Machine Language (AML), is a compact, tokenized, abstract type of machine language.

If you want see more than about for ACPI you can check that: [UEFI.ORG](https://uefi.org/sites/default/files/resources/ACPI_Spec_6_4_Jan22.pdf)

DSDT's & SSDT's are tables present in your firmware that outline hardware devices like USB controllers, CPU threads, embedded controllers, system clocks and such. A DSDT(Differentiated System Description Table) can be seen as the body holding most of the info with smaller bits of info being passed by the SSDT(Secondary System Description Table).

The following items are required for motherboards after B300 series

- ACPI/SSDT-EC-USBX.aml
- ACPI/SSDT-AWAC.aml
- ACPI/SSDT-RHUB.aml
- SSDT-SBUS-MCHC.aml (FOR SMBUS)
- ACPU/SSDT-GPU-DISABLE.aml (ONLY FOR RTX 4000 SERIES USERS)

Below are also the configurations that should be used for USB port processes on the MSI B760 motherboard. If you are going to install directly on the same model, you can use the USBMap.kext file defined in the configuration file and the SSDT-EC-USBX.aml configuration. If you are trying to install on a different motherboard, please make sure you have mapped to your motherboard model. Before proceeding, make sure your power management is properly configured and XCPM is installed. For detailed information see:

- [USBMapping Method 1](https://dortania.github.io/OpenCore-Post-Install/usb/manual/manual.html#usb-mapping-the-manual-way)
- [USBMapping 2 Method **[Highly Recommended 🌟]**](https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html)

(Don't forget! All Mac hardware only has 15 USB ports.)

### 2.2 Kexts (Kernel Extensions)

The so-called kernel extension file is actually what we usually call a driver.

The following two drivers are required

`Kexts/VirtualSMC.kext`
`Kexts/Lilu.kext`
The following two drivers are used to monitor CPU temperature and fan speed. They are suitable for both desktop and laptop computers, but can only be used under Intel CPUs.

`Kexts/SMCProcessor.kext`
`Kexts/SMCSuperIO.kext`
The driver above is the using for monitoring Intel CPU temperature driver. Its stable operation depends on having XCPM installed.

`Kexts/CPUFriend.kext`
This kernel extension is produced with the CPUFriend.command file. After performing all power management operations in order, you should check the operating frequency range of your processor with this kext. `CPUFriendDataProvider` is a subpath of this kext. Don't change the order.

**Default Value equal is 2.50-3.00 GHz Core AVG**

For more information about CPUFriend, visit this link.
[CPUFriend on Github](https://github.com/corpnewt/CPUFriendFriend)

<p align="center">
<img src='https://raw.githubusercontent.com/barisalby/gist/main/images/Hackintosh-UEFI-MSI-B760-13900K/7207740433699.png'>
</p>

`Kexts/RestrictEvents.kext`
The following driver is for CPUName Commander.

`Kexts/WhateverGreen.kext`
The following driver is a sound card driver that supports most onboard sound card drivers.

`Kexts/AppleALC.kext`
AppleALC is a kernel extension that will provide native support to your audio driver. If you have a B760 motherboard, do not change the configuration in the config.plist file. Otherwise try using different layout-ids.

Check here to view supported codecs.
[Audio Layout ID's](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)

`Kexts/USBInjectAll.kext` [RehabMan OS-X-USB-Inject-All](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)
If you are using a different motherboard model, implement this instead of `USBMap.kext`. Then provide your mapping according to the guide mentioned above. If you don't you can download latest version of USBInjectAll.kext.

`Kexts/AirportItlwm.kext`
`Kexts/IntelBluetoothFirmware.kext` Check [OpenIntelWireless](https://github.com/OpenIntelWireless/itlwm/releases)

### 2.3 PCI-Root Locations

<p align="center">
<img src='https://raw.githubusercontent.com/barisalby/gist/main/images/Hackintosh-UEFI-MSI-B760-13900K/5109602835010.png'>
</p>
The PCI location paths of the components on your motherboard may differ from the paths in the config.plist in the repository you pulled. For the most accurate PCI root location path mapping, you can get help from the Device Manager page in your system's Windows operating system, if available. Alternatively, you can try export log output of PCI Devices via `OpenShell.efi`.

For this reason you may sure completely deleted all PCI devices on config.plist files.

**If you are using OpenShell, make sure that the log you have obtained is as follows.**

```bash
Mapping table
      FS8: Alias(s):HD2b:;BLK13:
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14-01-00-C0-6B-A7-79-64)/HD(1,GPT,3A38FD13-4F67-4DDF-BEAA-60904393A700,0x800,0x32000)
      FS9: Alias(s):HD2d:;BLK15:
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14-01-00-C0-6B-A7-79-64)/HD(3,GPT,39567AA6-AFDA-4912-B515-D054469F6EA2,0x3A800,0x74594498)
     FS10: Alias(s):HD2e:;BLK16:
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14-01-00-C0-6B-A7-79-64)/HD(4,GPT,59AFA98E-F8EF-4EAB-A14F-EF25476A9493,0x745CF000,0x136800)
      FS0: Alias(s):HD0c:;BLK2:
          PciRoot(0x0)/Pci(0x1A,0x0)/Pci(0x0,0x0)/NVMe(0x1,A6-03-00-C0-6B-A7-79-64)/HD(2,GPT,3C500731-5D7E-4600-A472-B3050B66F1B2,0x8000,0x746FE000)
      FS1: Alias(s):HD1b:;BLK4:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(1,GPT,A11C6869-6ED1-47E8-B664-38877382C917,0x28,0x64000)
      FS5: Alias(s):HD1c:;BLK9:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7-0B7C-49F3-9147-01F4042E6842,68202B482DACBB4A8C5EA6963C5E6994)
      FS7: Alias(s):HD1c:;BLK11:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7-0B7C-49F3-9147-01F4042E6842,B9E14ADAF3BD044BB5C0D6D46812A156)
      FS3: Alias(s):HD1c:;BLK7:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7-0B7C-49F3-9147-01F4042E6842,51959A5C2C44F441BAD1049C94321665)
      FS6: Alias(s):HD1c:;BLK10:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7-0B7C-49F3-9147-01F4042E6842,98E80C70DCC23343AC6ECB0FEDB4E659)
      FS4: Alias(s):HD1c:;BLK8:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7-0B7C-49F3-9147-01F4042E6842,6286A1009BA9BB40BBA7E9BABA9600A3)
      FS2: Alias(s):HD1c:;BLK6:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7-0B7C-49F3-9147-01F4042E6842,477EC8866153A44F99B35F6B80A0AF03)
    BLK12: Alias(s):
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14-01-00-C0-6B-A7-79-64)
    BLK14: Alias(s):
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14-01-00-C0-6B-A7-79-64)/HD(2,GPT,A02AF1EB-B559-414E-9A2A-87A0F382A8E6,0x32800,0x8000)
     BLK0: Alias(s):
          PciRoot(0x0)/Pci(0x1A,0x0)/Pci(0x0,0x0)/NVMe(0x1,A6-03-00-C0-6B-A7-79-64)
     BLK1: Alias(s):
          PciRoot(0x0)/Pci(0x1A,0x0)/Pci(0x0,0x0)/NVMe(0x1,A6-03-00-C0-6B-A7-79-64)/HD(1,GPT,552608A5-29A7-49AF-8D18-CA76D7BBBA8E,0x22,0x7FDE)
     BLK3: Alias(s):
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)
     BLK5: Alias(s):
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22-01-00-C0-6B-A7-79-64)/HD(2,GPT,47447AD3-B50A-4D81-96CF-A73CE50F4022,0x64028,0x746A2D60)
```

# 3. SMBIOS

All SMBIOS information in config file has been removed for security. Please use the GenSMBIOS tool to obtain an appropriate SMBIOS output for your system. Since the Intel i9-13900k is not natively supported on macOS, it must be emulated to the 10th Gen. At this point, make sure that the output you receive from GenSMBIOS emulates the following device types.

- iMacPro1,1.
- MacPro7.1

# 4. Benchmark Results

[RX580 Benchmark on GeekBench 6](https://barisalby.github.io/benchmarks/RX580.html)

[i9-13900k Benchmark on GeekBench 6](https://barisalby.github.io/benchmarks/13900K.html)

**IMPORTANT**: The i9-13900k test results give lower values compared to the results of a 13900k running at full performance. This is due to the fact that the processor is not run at full speed in line with user preference. Overclock your CPUFriend settings for truly satisfying results.

Enjoy it.
