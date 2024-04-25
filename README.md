<h1 class="remove">Hackintosh-UEFI-MSI-B760-13900K</h1>

<p class="remove">This repo contains 
  configuration and bootloader files that fully support macOS 14.4 
  Sonoma (23E214) with on MSI B760 motherboard and 13900k processor.
</p>

<div class="frame" align="center">
  <img
    alt="System Overview"
    src="https://arg0wak.github.io/gist/images/Hackintosh-UEFI-MSI-B760-13900K/71785754712151.png"
  />
</div>

<h1>1. Preparation</h1>
<h3>1.1 Environment:</h3>
<table>
  <tbody>
    <tr>
      <th align="center">
        <img class="remove" width="441" height="1" />
        <p>Device</p>
      </th>
      <th align="center">
        <img class="remove" width="441" height="1" />
        <p>Working</p>
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
  </tbody>
</table>

<div class="frame" align="center">
  <img
    alt="Hardware List"
    src="https://arg0wak.github.io/gist/images/Hackintosh-UEFI-MSI-B760-13900K/6379423821685.png"
  />
</div>

<h3>1.2 BIOS</h3>
<table>
  <tbody>
    <tr>
      <th align="center">
        <img class="remove" width="441" height="1" />
        <p>Property</p>
      </th>
      <th align="center">
        <img class="remove" width="441" height="1" />
        <p>Status</p>
      </th>
    </tr>
    <tr>
      <td>Secure Boot</td>
      <td><strong>Disabled</strong></td>
    </tr>
    <tr>
      <td>Fast Boot</td>
      <td><strong>Disabled</strong></td>
    </tr>
    <tr>
      <td>MSI Fast Boot</td>
      <td><strong>Disabled</strong></td>
    </tr>
    <tr>
      <td>Re-Size BAR Support</td>
      <td><strong>Disabled</strong></td>
    </tr>
    <tr>
      <td>Initiate Graphic Adapter</td>
      <td><strong>[PEG]</strong></td>
    </tr>
    <tr>
      <td>Integrated Graphics Share Memory</td>
      <td><strong>[64M]</strong></td>
    </tr>
    <tr>
      <td>IGD Multi Monitor</td>
      <td><strong>Enabled</strong></td>
    </tr>
    <tr>
      <td>Discrete Thunderbolt(TM) Support</td>
      <td><strong>Disabled</strong></td>
    </tr>
    <tr>
      <td>XHCI Hand-Off</td>
      <td><strong>Enabled</strong></td>
    </tr>
    <tr>
      <td>Legacy USB Support</td>
      <td><strong>Enabled</strong></td>
    </tr>
    <tr>
      <td>BIOS CSM/UEFI Mode</td>
      <td><strong>[UEFI]</strong></td>
    </tr>
    <tr>
      <td>Above 4G Memory</td>
      <td><strong>Enabled</strong></td>
    </tr>
  </tbody>
</table>

<h1>2. ACPI (Advanced Configuration and Power Interface)</h1>
<h3>2.1 Introduction</h3>
<p>
  Advanced Configuration and Power Interface (ACPI) specification was developed
  to establish industry common interfaces enabling robust operating system
  (OS)-directed motherboard device configuration and power management of both
  devices and entire systems.
</p>
<p>
  The pseudo-code language, known as ACPI Machine Language (AML), is a compact,
  tokenized, abstract type of machine language.
</p>
<p>
  If you want see more than about for ACPI you can check that:
  <a
    href="https://uefi.org/sites/default/files/resources/ACPI_Spec_6_4_Jan22.pdf"
    >UEFI.ORG</a
  >
</p>
<p>
  DSDT's &amp; SSDT's are tables present in your firmware that outline hardware
  devices like USB controllers, CPU threads, embedded controllers, system clocks
  and such. A DSDT(Differentiated System Description Table) can be seen as the
  body holding most of the info with smaller bits of info being passed by the
  SSDT(Secondary System Description Table).
</p>
<p>The following items are required for motherboards later B300 series</p>
<ul>
  <li>ACPI/SSDT-EC-USBX.aml</li>
  <li>ACPI/SSDT-AWAC.aml</li>
  <li>ACPI/SSDT-RHUB.aml</li>
  <li>SSDT-SBUS-MCHC.aml (FOR SMBUS)</li>
  <li>ACPU/SSDT-GPU-DISABLE.aml (ONLY FOR RTX 4000 SERIES USERS)</li>
</ul>
<p>
  Below are also the configurations that should be used for USB port processes
  on the MSI B760 motherboard. If you are going to install directly on the same
  model, you can use the USBMap.kext file defined in the configuration file and
  the SSDT-EC-USBX.aml configuration. If you are trying to install on a
  different motherboard, please make sure you have mapped to your motherboard
  model. Before proceeding, make sure your power management is properly
  configured and XCPM is installed. For detailed information see:
</p>
<ul>
  <li>
    <a
      href="https://dortania.github.io/OpenCore-Post-Install/usb/manual/manual.html#usb-mapping-the-manual-way"
      >USBMapping Method 1</a
    >
  </li>
  <li>
    <a
      href="https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html"
      >USBMapping Method 2 <strong>[Highly Recommended 🌟]</strong></a
    >
  </li>
</ul>
<p>(Don't forget! All Mac hardware only has 15 USB ports.)</p>
<h3>2.2 Kexts (Kernel Extensions)</h3>
<p>
  The so-called kernel extension file is actually what we usually call a driver.
</p>
<p>The following two drivers are required</p>
<p>
  <code>Kexts/VirtualSMC.kext</code>
  <code>Kexts/Lilu.kext</code>
  The following two drivers are used to monitor CPU temperature and fan speed.
  They are suitable for both desktop and laptop computers, but can only be used
  under Intel CPUs.
</p>
<p>
  <code>Kexts/SMCProcessor.kext</code>
  <code>Kexts/SMCSuperIO.kext</code>
  The driver above is the using for monitoring Intel CPU temperature driver. Its
  stable operation depends on having XCPM installed.
</p>
<p>
  <code>Kexts/CPUFriend.kext</code> This kernel extension is produced with the
  CPUFriend.command file. After performing all power management operations in
  order, you should check the operating frequency range of your processor with
  this kext. <code>CPUFriendDataProvider</code> is a subpath of this kext. Don't
  change the order.
</p>
<p><strong>Default Value equal is 2.50-3.00 GHz Core AVG</strong></p>
<p>
  For more information about CPUFriend, visit this link.
  <a href="https://github.com/corpnewt/CPUFriendFriend">CPUFriend on Github</a>
</p>
<div class="frame" align="center">
  <img
    style="height: 650px"
    alt="Intel Power Gadget"
    src="https://arg0wak.github.io/gist/images/Hackintosh-UEFI-MSI-B760-13900K/7207740433699.png"
  />
</div>
<br/>
<p>
  <code>Kexts/RestrictEvents.kext</code> The following driver is for CPUName
  Commander.
</p>
<p>
  <code>Kexts/WhateverGreen.kext</code> The following driver is a sound card
  driver that supports most onboard sound card drivers.
</p>
<p>
  <code>Kexts/AppleALC.kext</code> AppleALC is a kernel extension that will
  provide native support to your audio driver. If you have a B760 motherboard,
  do not change the configuration in the config.plist file. Otherwise try using
  different layout-ids.
</p>
<p>
  Check here to view supported codecs.
  <a href="https://github.com/acidanthera/AppleALC/wiki/Supported-codecs"
    >Audio Layout ID's</a
  >
</p>
<p>
  <code>Kexts/USBInjectAll.kext</code>
  <a href="https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/"
    >RehabMan OS-X-USB-Inject-All</a
  >
  If you are using a different motherboard model, implement this instead of
  <code>USBMap.kext</code>. Then provide your mapping according to the guide
  mentioned above. If you don't you can download latest version of
  USBInjectAll.kext.
</p>
<p>
  <code>Kexts/AirportItlwm.kext</code>
  <code>Kexts/IntelBluetoothFirmware.kext</code> Check
  <a href="https://github.com/OpenIntelWireless/itlwm/releases"
    >OpenIntelWireless</a
  >
</p>
<h3>2.3 PCI-Root Locations</h3>
<div class="frame" align="center">
  <img
    src="https://arg0wak.github.io/gist/images/Hackintosh-UEFI-MSI-B760-13900K/5109602835010.png"
  />
</div>
<br/>
<p>The PCI location paths of the components on your motherboard may differ from the
paths in the config.plist in the repository you pulled. For the most accurate
PCI root location path mapping, you can get help from the Device Manager page in
your system's Windows operating system, if available. Alternatively, you can try
export log output of PCI Devices via</p>
<code>OpenShell.efi</code>.
<br /><br />
<p>
  For this reason you may sure completely deleted all PCI devices on
  config.plist files.
</p>
<p>
  <br/>
  <strong
    >If you are using OpenShell, make sure that the log you have obtained is as
    follows.</strong
  >
</p>
<pre><code class="lang-bash">Mapping table
      FS8: Alias(s):HD2b:;BLK13:
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(1,GPT,3A38FD13<span class="hljs-string">-4</span>F67<span class="hljs-string">-4</span>DDF-BEAA<span class="hljs-string">-60904393</span>A700,0x800,0x32000)
      FS9: Alias(s):HD2d:;BLK15:
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(3,GPT,39567AA6-AFDA<span class="hljs-string">-4912</span>-B515-D054469F6EA2,0x3A800,0x74594498)
     FS10: Alias(s):HD2e:;BLK16:
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(4,GPT,59AFA98E-F8EF<span class="hljs-string">-4</span>EAB-A14F-EF25476A9493,0x745CF000,0x136800)
      FS0: Alias(s):HD0c:;BLK2:
          PciRoot(0x0)/Pci(0x1A,0x0)/Pci(0x0,0x0)/NVMe(0x1,A6<span class="hljs-string">-03</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,3C500731<span class="hljs-string">-5</span>D7E<span class="hljs-string">-4600</span>-A472-B3050B66F1B2,0x8000,0x746FE000)
      FS1: Alias(s):HD1b:;BLK4:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(1,GPT,A11C6869<span class="hljs-string">-6</span>ED1<span class="hljs-string">-47</span>E8-B664<span class="hljs-string">-38877382</span>C917,0x28,0x64000)
      FS5: Alias(s):HD1c:;BLK9:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7<span class="hljs-string">-0</span>B7C<span class="hljs-string">-49</span>F3<span class="hljs-string">-9147</span><span class="hljs-string">-01</span>F4042E6842,68202B482DACBB4A8C5EA6963C5E6994)
      FS7: Alias(s):HD1c:;BLK11:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7<span class="hljs-string">-0</span>B7C<span class="hljs-string">-49</span>F3<span class="hljs-string">-9147</span><span class="hljs-string">-01</span>F4042E6842,B9E14ADAF3BD044BB5C0D6D46812A156)
      FS3: Alias(s):HD1c:;BLK7:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7<span class="hljs-string">-0</span>B7C<span class="hljs-string">-49</span>F3<span class="hljs-string">-9147</span><span class="hljs-string">-01</span>F4042E6842,51959A5C2C44F441BAD1049C94321665)
      FS6: Alias(s):HD1c:;BLK10:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7<span class="hljs-string">-0</span>B7C<span class="hljs-string">-49</span>F3<span class="hljs-string">-9147</span><span class="hljs-string">-01</span>F4042E6842,98E80C70DCC23343AC6ECB0FEDB4E659)
      FS4: Alias(s):HD1c:;BLK8:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7<span class="hljs-string">-0</span>B7C<span class="hljs-string">-49</span>F3<span class="hljs-string">-9147</span><span class="hljs-string">-01</span>F4042E6842,6286A1009BA9BB40BBA7E9BABA9600A3)
      FS2: Alias(s):HD1c:;BLK6:
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)/VenMedia(BE74FCF7<span class="hljs-string">-0</span>B7C<span class="hljs-string">-49</span>F3<span class="hljs-string">-9147</span><span class="hljs-string">-01</span>F4042E6842,477EC8866153A44F99B35F6B80A0AF03)
    BLK12: Alias(s):
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)
    BLK14: Alias(s):
          PciRoot(0x0)/Pci(0x6,0x0)/Pci(0x0,0x0)/NVMe(0x1,14<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,A02AF1EB-B559<span class="hljs-string">-414</span>E<span class="hljs-string">-9</span>A2A<span class="hljs-string">-87</span>A0F382A8E6,0x32800,0x8000)
     BLK0: Alias(s):
          PciRoot(0x0)/Pci(0x1A,0x0)/Pci(0x0,0x0)/NVMe(0x1,A6<span class="hljs-string">-03</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)
     BLK1: Alias(s):
          PciRoot(0x0)/Pci(0x1A,0x0)/Pci(0x0,0x0)/NVMe(0x1,A6<span class="hljs-string">-03</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(1,GPT,552608A5<span class="hljs-string">-29</span>A7<span class="hljs-string">-49</span>AF<span class="hljs-string">-8</span>D18-CA76D7BBBA8E,0x22,0x7FDE)
     BLK3: Alias(s):
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)
     BLK5: Alias(s):
          PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)/NVMe(0x1,22<span class="hljs-string">-01</span><span class="hljs-string">-00</span>-C0<span class="hljs-string">-6</span>B-A7<span class="hljs-string">-79</span><span class="hljs-string">-64</span>)/HD(2,GPT,47447AD3-B50A<span class="hljs-string">-4</span>D81<span class="hljs-string">-96</span>CF-A73CE50F4022,0x64028,0x746A2D60)
</code></pre>
<h1>3. SMBIOS</h1>
<p>
  All SMBIOS information in config file has been removed for security. Please
  use the GenSMBIOS tool to obtain an appropriate SMBIOS output for your system.
  Since the Intel i9-13900k is not natively supported on macOS, it must be
  emulated to the 10th Gen. At this point, make sure that the output you receive
  from GenSMBIOS emulates the following device types.
</p>
<ul>
  <li>iMacPro1,1.</li>
  <li>MacPro7.1</li>
</ul>
<h1>4. Benchmark Results</h1>
<p>
  <a href="https://arg0wak.github.io/benchmarks/RX580.html"
    >RX580 Benchmark on GeekBench 6</a
  >
</p>
<p>
  <a href="https://arg0wak.github.io/benchmarks/13900K.html"
    >i9-13900k Benchmark on GeekBench 6</a
  >
</p>
<p>
  <strong>IMPORTANT</strong>: The i9-13900k test results give lower values
  compared to the results of a 13900k running at full performance. This is due
  to the fact that the processor is not run at full speed in line with user
  preference. Overclock your CPUFriend settings for truly satisfying results.
</p>
<p>Enjoy it.</p>
