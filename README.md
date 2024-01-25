## Rog-STRIX-Z590-A-Gaming-WIFI-II-Hackintosh

Install macOS Ventura on [ORG Z590-A Gaming Wifi II](https://rog.asus.com/motherboards/rog-strix/rog-strix-z590-a-gaming-wifi-ii-model/) Mainboard with 10th Gen Intel CPU.

![snaphot](docs/snaphot.png)

---

### Information 

- macOS: [Ventura](https://www.apple.com/macos/ventura/)
- bootloader: [OpenCore 0.9.7](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.9.7)

---


### Hardware

| Component    | Variant                   | Link                                                                                                                                         |
|:------------:|:-------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------:|
| Mainboard    | ORG Z590-A Gaming Wifi II | [www.asus.com](https://rog.asus.com/motherboards/rog-strix/rog-strix-z590-a-gaming-wifi-ii-model/)                                           |
| Processor    | Intel Core i9 10900K      | [ark.intel.com](https://ark.intel.com/content/www/us/en/ark/products/199332/intel-core-i910900k-processor-20m-cache-up-to-5-30-ghz.html)     |
| DDR4 RAM     | Corsair 128GB             | [www.corsair.com](https://www.corsair.com/ja/zh/%E7%B1%BB%E5%88%AB/%E4%BA%A7%E5%93%81/%E5%86%85%E5%AD%98/VENGEANCE-LPX/p/CMK128GX4M4A2666C16)|
| NVMe SSD     | Kingston 2TB              | [www.kingston.com](https://www.kingston.com.cn/en/ssd/dc1000b-data-center-boot-ssd)                                                          |
| Graphics     | AMD Radeon RX 6900xt      | [www.amd.com](https://www.amd.com/en/products/graphics/amd-radeon-rx-6900-xt)                                                                |
| WiFi / BT    | Intel AX201/BCM94360CD    | [www.intel.com](https://www.intel.com/content/www/us/en/products/sku/130293/intel-wifi-6-ax201-gig/specifications.html)                      |


---

### BIOS 

#### firmware 

[Version 2001](https://rog.asus.com.cn/motherboards/rog-strix/rog-strix-z590-a-gaming-wifi-ii-model/helpdesk_bios/)

#### settings

```
Advanced
  - CPU Configuration
    - Intel (VMX) Virtualization Technology: disable
    - Hyper-Threading: Enabled
  - System Agent (SA)-Configuration
    - Graphics Configuration
      - iGPU Multi-Monitor: Enabled
      - DVMT Pre-Allocated: 64M
  - Thunderbolt(TM) Configuration
    - Discrete Thunderbolt(TM) Support: Disabled
  - PCI Subsystem Settings
    - Above 4G Decoding: Enabled
  - USB Configuration
    - Legacy USB Support: Enabled
    - XHCI Hand-off: Enabled
  - PCH Storge Configuration
    - SATA Mode Selection: AHCI
  - Trusted Computing
    - Security Device Support: Disable
Boot
  - CSM (Compatibility Support Module)
    - Launch CSM: Disabled
  - Secure Boot
    - OS Type : Other OS
    - Secure Boot Mode : Custom
  - Boot Configuration
    - Fast Boot: Disabled

```

- enable/disable VMX 

if enable VMX need set  EnableVmx to YES in UEFI–Quirks

![enable VMX OC config](docs/enable_vmx_oc_config.png)

![enable VMX](docs/bios/enable_vmx.BMP)

- enable Hyper-Threading

![enable Hyper-Threading](docs/bios/enable_hyper-threading.BMP)

- enable iGPU 

If your cpu has core display and the core is macos supported, then you can refer to the guide to configure it.

[Guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#deviceproperties)

- diable Thunderbolt

![Thunderbolt Configuration](docs/bios/thunderbolt_configuration.BMP)

![disable Thunderbolt support](docs/bios/disable_thunderbolt_support.BMP)


- enable Above 4G Decoding

![pci subsystem settings](docs/bios/pci_subsystem_settings.BMP)

When enabling Above4G, Resizable BAR Support may become an available on some Z490 and newer motherboards. Please ensure that Booter -> Quirks -> ResizeAppleGpuBars is set to 0 if this is enabled.

![enable above 4G decoding](docs/bios/enable_above_4G_decoding.BMP)

![ResizeAppleGpuBars](docs/ResizeAppleGpuBars.png)


- enable legacy usb support and xhci hand-off

![usb configuration](docs/bios/usb_configuration.BMP)

![enable legacy usb support](docs/bios/enable_legacy_usb_support.BMP)

![enable xhci hand-off](docs/bios/enable_xhci_hand-off.BMP)

- select AHCI mode

![pch storage configuration](docs/bios/pch_storage_configuration.BMP)

![sata mode selection](docs/bios/sata_mode_selection.BMP)

- disable CSM

![CSM](docs/bios/csm.BMP)

![disable launch CSM](docs/bios/disable_launch_csm.BMP)

- disable Secure Boot

![secure boot](docs/bios/secure_boot.BMP)

![secure boot mode](docs/bios/secure_boot_mode.BMP)


- disable Fast Boot

![boot configuration](docs/bios/boot_configuration.BMP)

![disable fast boot](docs/bios/disable_fast-boot.BMP)

- disable Trusted Computing

If your computer only has macos, or the version of windows installed is less than windows 11 , then it is recommended to turn it off.

![disable trusted computing](docs/bios/disbale_trusted_computing.BMP)

- disable Wi-Fi & Bluetooth

If you have a compatible wireless card and you want to disable the on-board wifi and bluetooth, then you need to configure it as shown.

![disable WiFi & bluetooth](docs/bios/disable_wifi_bt.BMP)


If you are using intel 11th generation CPU, please disable integrated graphics in BIOS. 
And remove integrated graphics device properties from config.plist.

```
disable iGPU : iGPU Multi-Monitor: Disabled
```

Remove:

`DeviceProperties->Add`

```xml
<key>PciRoot(0x0)/Pci(0x2,0x0)</key>
<dict>
  <key>AAPL,ig-platform-id</key>
  <data>AwCRPg==</data>
  <key>device-id</key>
  <data>mz4AAA==</data>
  <key>enable-metal</key>
  <data>AQAAAA==</data>
</dict>
```

If got black screen in Windows when booting from OpenCore also need remove it.



---

### EFI 

#### Config

##### Tools

- [ProperTree](https://github.com/corpnewt/ProperTree)

- [OpencoreConfigurator](https://mackie100projects.altervista.org/download-opencore-configurator/)   

- [Sublime](https://www.sublimetext.com/download)

- [OCAuxiliaryTools (support windows)](https://github.com/ic005k/OCAuxiliaryTools)

##### Debug

[OpenCore Install Guide Debug Config Changes](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/debug.html#config-changes)

OC boot core files, there is a debug version, if you run into OC boot install black apple system failed, infinite stuck, infinite reboot error, you can download the debug version, turn on the logging, and then go to the debug txt in the EFI partition to take a closer look at the problems encountered in the installation, as well as the error location and code.

[OpenCore Debug](https://github.com/acidanthera/OpenCorePkg/releases)

![OpenCore Debug](docs/opencore-debug.png)


The modifications are as follows

Config-Misc-Debug: debugging options

AppleDebug: when checked, boot.efi debug logs are saved in OpenCore logs. Fill in YES

ApplePanic: save macOS kernel crash logs to OpenCore root partition, use it or not according to your need. Fill in YES/Ture

DisableWatchDog: Generally do not need to check, very few motherboards may need to check. Fill in NO/False

DisplayDelay: display delay, fill in 0.

DisplayLevel: display level, fill in: 2147483714

Target: Target, usually fill in 67.

For other values of Target, please refer to: 0: disable logging, 3: allow screen output logging, 19: allow screen output logging of UEFI variables, 65: generate log file opencore-YYYY-MM-DD-HHMMSS.txt in the root directory of the ESP partition, but do not display logs on the screen.

![OpencoreConfigurator](docs/opencore-debug-oc-config.png)

![ProperTree](docs/opencore-debug-pt-config.png)

```xml
  <key>Misc</key>
  <dict>
    <key>Debug</key>
    <dict>
      <key>AppleDebug</key>
      <true/>
      <key>ApplePanic</key>
      <true/>
      <key>DisableWatchDog</key>
      <true/>
      <key>DisplayDelay</key>
      <integer>0</integer>
      <key>DisplayLevel</key>
      <integer>2147483714</integer>
      <key>LogModules</key>
      <string>*</string>
      <key>SysReport</key>
      <false/>
      <key>Target</key>
      <integer>67</integer>
    </dict>
  </dict>
</dict>
```

![Debug File](docs/opencore-debug-file.png)

- Disable Debug

Use a stable version of OpenCore

![OpenCore disable debug](docs/opencore-disable-debug-oc-config.png)

#### 11 Gen CPU

For 11th gen CPUs, you can refer to the [i911900k](https://github.com/cuihairu/ROG-STRIX-Z590-A-i910900K-RX6900XT-Hackintosh/tree/i911900KF)branch and the [i7-11700k-RX6600XT](https://github.com/cuihairu/ROG-STRIX-Z590-A-i910900K-RX6900XT-Hackintosh/tree/i7-11700k-RX6600XT) branch.

i5-10x00 :

```xml
<key>Emulate</key>
<dict>
  <key>Cpuid1Data</key>
  <data>6wYJAAAAAAAAAAAAAAAAAA==</data>
  <key>Cpuid1Mask</key>
  <data>/////wAAAAAAAAAAAAAAAA==</data>
  <key>DummyPowerManagement</key>
  <false/>
  <key>MaxKernel</key>
  <string></string>
  <key>MinKernel</key>
  <string></string>
</dict>
```

```xml
<key>Emulate</key>
<dict>
   <key>Cpuid1Data</key>
   <data>6gYJAAAAAAAAAAAAAAAAAA==</data>
   <key>Cpuid1Mask</key>
   <data>/////wAAAAAAAAAAAAAAAA==</data>
   <key>DummyPowerManagement</key>
   <false/>
   <key>MaxKernel</key>
   <string></string>
   <key>MinKernel</key>
   <string></string>
</dict>
```

i7-10700k:

```xml
<key>Emulate</key>
<dict>
  <key>Cpuid1Data</key>
  <data>VQYKAAAAAAAAAAAAAAAAAA==</data>
  <key>Cpuid1Mask</key>
  <data>/////wAAAAAAAAAAAAAAAA==</data>
  <key>DummyPowerManagement</key>
  <false/>
  <key>MaxKernel</key>
  <string></string>
  <key>MinKernel</key>
  <string></string>
</dict>
```




#### Kexts

| Kext                                 | Version      | Author                                                                                                             |
|:------------------------------------:|:------------:|:------------------------------------------------------------------------------------------------------------------:|
| XHCI-unsupported.kext                | v0.9.2       | [RehabMan/OS-X-USB-Inject-All](https://github.com/RehabMan/OS-X-USB-Inject-All/tree/master/XHCI-unsupported.kext)  |
| Lilu.kext                            | v1.6.7       | [acidanthera/Lilu](https://github.com/acidanthera/Lilu/releases)                                                   |
| SMCProcessor.kext                    | v1.3.2       | [acidanthera/VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)                                       |
| SMCSuperIO.kext                      | v1.3.2       | [acidanthera/VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)                                       |
| VirtualSMC.kext                      | v1.3.2       | [acidanthera/VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)                                       | 
| RadeonSensor.kext                    | v1.3.0       | [ChefKissInc/RadeonSensor](https://github.com/ChefKissInc/RadeonSensor/releases)                                   |
| SMCRadeonGPU.kext                    | v1.3.0       | [ChefKissInc/RadeonSensor](https://github.com/ChefKissInc/RadeonSensor/releases)                                   |
| RestrictEvents.kext                  | v1.1.3       | [acidanthera/RestrictEvents](https://github.com/acidanthera/RestrictEvents)                                        |
| NVMeFix.kext                         | v1.1.1       | [acidanthera/NVMeFix](https://github.com/acidanthera/NVMeFix)                                                      |                                                           
| AppleIGC                             | v1.4.0       | [SongXiaoXi/AppleIGC](https://github.com/SongXiaoXi/AppleIGC)                                                      |
| AirpotItlwm.kext                     | v2.2.0       | [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm/releases)                                     |
| IntelBluetootchFirmware.kext         | v2.3.0       | [OpenIntelWireless/IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)   |
| IntelBTPatcher.kext                  | v2.6.8       | [acidanthera/BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases)                                   |
| BlueToolFixup.kext                   | v2.6.8       | [acidanthera/BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases)                                   |
| FeatureUnlock.kext                   | v1.1.5       | [acidanthera/FeatureUnlock](https://github.com/acidanthera/FeatureUnlock/releases)                                 |
| USBToolBox.kext                      | v1.1.1       | [USBToolBox/kext](https://github.com/USBToolBox/kext/releases)                                                     |
| UTBMap.kext                          | v1.1.1       | [USBToolBox/tool](https://github.com/USBToolBox/tool)                                                              |     
| WhateverGreen.kext                   | v1.6.6       | [acidanthera/WhateverGreen/](https://github.com/acidanthera/WhateverGreen/releases)                                |
| AirportBrcmFixup.kext                | v2.1.8       | [acidanthera/AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)                           |



App:


| App                                  | Version      | Author                                                                                                             |
|:------------------------------------:|:------------:|:------------------------------------------------------------------------------------------------------------------:|
| RadeonGadget                         | v1.3.0       | [ChefKissInc/RadeonSensor](https://github.com/ChefKissInc/RadeonSensor/releases)                                   |
| HeliPort                             | v.1.4.1      | [OpenIntelWireless/HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases)                               |

---

### Utils

| Name                                 | Version      | Author                                                                                                             |
|:------------------------------------:|:------------:|:------------------------------------------------------------------------------------------------------------------:|
| ioreg                                | v3.0.2       | [khronokernel/IORegistryClone](https://github.com/khronokernel/IORegistryClone/blob/master/ioreg-302.zip)          |
| Hackintool                           | v4.0.3       | [benbaker76/Hackintool](https://github.com/benbaker76/Hackintool/releases)                                         |
| OpenCore Configurator                | v2.74.10     | [occ](https://mackie100projects.altervista.org/download-opencore-configurator/)                                    |
| MonitorControl                       | v4.2.0       | [MonitorControl](https://github.com/MonitorControl/MonitorControl/releases)                                        |

---


### Update macOS

Check the official update-guide: [OpenCore-Post-Install/update](https://dortania.github.io/OpenCore-Post-Install/universal/update.html)

1. Backup
   - Full system backup with `Time Machine` or similar software
   - Copy current EFI to OpenCore USB-Drive for recovery purpose
2. Download
   - Latest version of OpenCore and replace files in EFI
   - Updates for all installed kexts and replace in EFI
3. Reboot
   - Boot with updated OpenCore version and kexts
   - If the system doesn't boot, use OpenCore USB-Drive to roll back
4. Update
   - Start macOS Update from `System Settings` -> `Software Update`
   - With OpenCore the update process should work automatically
   - If `Software Update` shows `Mac version is up to date`, download macOS Installer from AppStore and start the update manually

If the system doesn't boot, try to fix the problem or revert to the latest EFI or system-backup.
