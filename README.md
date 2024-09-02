<br />
<div align=center>
    <div align="center">
        <img src="https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Large.png" alt="OpenCore Logo" width="256"/>
    </div>
  <h4>OpenCore EFI for Lenovo Ideapad 120S 11-IAP</a></h4>
</div>

___

> [!IMPORTANT]  
> This EFI is intended to be used for reference only, and cannot be used as-is. Even for identical hardware, you will need to generate your own [SMBIOS](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/skylake.html#platforminfo) data, [USB mapping](https://dortania.github.io/OpenCore-Post-Install/usb/), and [ACPI patches](https://dortania.github.io/Getting-Started-With-ACPI/ssdt-methods/ssdt-prebuilt.html#laptop-skylake-and-kaby-lake) to ensure proper functionality.

> [!CAUTION]
> Please follow the [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) to create your own EFI.

___

## 🖥️ Hardware

| Component         | Model                            |
| ----------------- | -------------------------------- |
| CPU               | Intel Celeron N3350              |
| iGPU              | Intel HD Graphics 500            |
| Motherboard       | Lenovo LNVNB161216               |
| RAM               | 4GB LPDDR4 2133MHz               |
| Storage           | 32GB SanDisk DF4032 eMMC         |
| Audio             | Realtek ALC269                   |
| Wi-Fi + Bluetooth | Intel Dual Band Wireless-AC 3165 |
| BIOS Version      | 6GCN32WW                         |

## 📝 Notes

### 🛠️ BIOS Settings

> [!NOTE]
> This is not an exhaustive list of required BIOS settings. Please refer to the [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/skylake.html#intel-bios-settings) for more information.

| Setting                               | Value    |
| ------------------------------------- | -------- |
| Secure Boot                           | Disabled |
| Intel Virtualization Technology (VT)  | Enabled  |

### ⚒️ What works

- [x] USB
- [x] NVRAM
- [x] Wi-Fi + Bluetooth
- [x] Battery Status
- [x] Keyboard
- [x] eMMC

### ⚠️ What doesn't work

- [ ] iGPU (Intel HD Graphics 500)
- [ ] AVX, AVX2, and FMA instructions
  - The Celeron N3350 does not support these instructions.
- [ ] Sleep
- [ ] Audio
  - There are 60 layout IDs to test, I got up to layout 20 before giving up with no success.
- [ ] Touchpad
  - The touchpad is I2C HID, however there is no GPI0 in the ACPI tables.
- [ ] Brightness Control
- [ ] HDMI Output (untested)
- [ ] SD Card Reader (untested)
- [ ] EasyCamera

#### 🖼️ Graphics Acceleration

The Intel HD Graphics 500 iGPU is a GT1 GPU. macOS has only ever supported GT2 and above iGPUs. Unfortunately forcing the Skylake iGPU drivers to attach to the Apollo Lake iGPU results in a hang during the end of the first stage boot. This is likely caused by the graphics stack making use of AVX, AVX2, and/or FMA instructions, as all supported iGPUs have at least AVX support on the CPU side. It ***may*** be possible to patch the graphics stack to avoid/replace these instructions. See below for more information on parts of the stack in macOS Catalina.

| Extension                             | Notes                                                                                                                                                                                                                                                       |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AppleIntelSKLGraphics.kext            | ✅ The Skylake Graphics Kernel Extension contains no use of these instructions                                                                                                                                                                              |
| AppleIntelSKLGraphicsFramebuffer.kext | ✅ The Skylake Graphics Framebuffer Kernel Extension contains no use of these instructions                                                                                                                                                                  |
| AppleIntelSKLGraphicsGLDriver.bundle  | ❎ The Skylake Graphics OpenGL Driver itself contains ~296 calls to these instructions; however the majority of these are AVX 512, and additionally the CPU capability appears to be checked before being called for most calls                             |
| AppleIntelSKLGraphicsMTLDriver.bundle | ✅ The Skylake Graphics Metal Driver contains no use of these instruction                                                                                                                                                                                   |
| AppleIntelSKLGraphicsVADriver.bundle  | ✅ The Skylake Graphics Video Acceleration Driver contains no use of these instructions                                                                                                                                                                     |
| AppleIntelSKLGraphicsVAME.bundle      | ✅ The Skylake Graphics Video Acceleration Metal Engine contains no use of these instructions                                                                                                                                                               |
| AppleIntelGraphicsShared.bundle       | ❎ The Shared Graphics bundle, specifically just libigc (Intel Graphics Compiler) contains ~170 calls to these instructions; and while a majority of these are AVX 512, the CPU capability does not appear to be checked before being called for most calls |

## 📦 Versions

| Component                                                                                                         | Version |
| ----------------------------------------------------------------------------------------------------------------- | ------- |
| [OpenCore](https://github.com/acidanthera/OpenCorePkg/)                                                           | 1.0.1   |
| [AppleALC](https://github.com/acidanthera/AppleALC)                                                               | 1.9.1   |
| [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM)                                                      | 2.6.8   |
| [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys/)                                                  | 1.0.3   |
| [CPUFriend](https://github.com/acidanthera/CPUFriend)                                                             | 1.2.8   |
| [ECEnabler](https://github.com/1Revenger1/ECEnabler/)                                                             | 1.0.5   |
| [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC)                                                         | 0.1.2   |
| [FeatureUnlock](https://github.com/acidanthera/FeatureUnlock/)                                                    | 1.1.6   |
| [HoRNDIS](https://github.com/Edwardwich/HoRNDIS)                                                                  | 9.3     |
| [Itlwm](https://github.com/OpenIntelWireless/itlwm/)                                                              | 2.3.0   |
| [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/)                            | 2.4.0   |
| [Lilu](https://github.com/acidanthera/Lilu)                                                                       | 1.6.8   |
| [MouSSE](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/) | 0.9.5   |
| [RestrictEvents](https://github.com/acidanthera/RestrictEvents)                                                   | 1.1.4   |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC)                                                           | 1.3.3   |
| [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/)                                                              | 2.8     |
| [VoodooPS2](https://github.com/acidanthera/VoodooPS2)                                                             | 2.3.5   |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen)                                                     | 1.6.7   |

## 📚 References

- [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [OpenCore Post-Install Guide](https://dortania.github.io/OpenCore-Post-Install/)
- [MouSSE](https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/)
- [OpenCore Legacy Patcher Patches Explained](https://dortania.github.io/OpenCore-Legacy-Patcher/PATCHEXPLAIN.html)
