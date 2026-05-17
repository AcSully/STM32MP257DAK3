# STM32MP257DAK3 Board Support Package

This is a STM32CubeMX-generated BSP (Board Support Package) for the **STM32MP257DAK3** microprocessor, targeting the **Alientek (正点原子) STM32MP257 development board**.

## Supported Boards

| Board | Configuration | Status |
|-------|---------------|--------|
| Alientek STM32MP257 | 1+8G (1GB DDR + 8GB eMMC) | ✅ Supported |
| Alientek STM32MP257 | 2+16G (2GB DDR + 16GB eMMC) | ✅ Supported |

## Hardware Overview

**SoC**: STM32MP257DAK3 (STM32MP2 Series)
- **Application Processor**: Dual-core Cortex-A35 @ up to 1.5GHz
- **Coprocessor**: Cortex-M33 @ 400MHz
- **GPU**: Vivante GC7000UL (3D/2D graphics)
- **Video**: Hardware H.264/H.265 codec

## Software Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Cortex-A35 (CA35)                        │
│   TF-A → OP-TEE → U-Boot → Linux (OpenSTLinux 6.6)         │
│   Device Trees: kernel / u-boot / tf-a / optee-os          │
├─────────────────────────────────────────────────────────────┤
│              IPCC1 (Inter-Processor Communication)          │
│                    OpenAMP / RPMSG                          │
├─────────────────────────────────────────────────────────────┤
│                    Cortex-M33 (CM33)                        │
│   STM32 HAL Driver + IPCC/UART4 (NonSecure Firmware)        │
│   TF-M (Trusted Firmware-M)                                 │
└─────────────────────────────────────────────────────────────┘
```

**OpenSTLinux Version**: `openstlinux-6.6-yocto-scarthgap-mpu-v25.06.11`

## Supported Peripherals

| Peripheral | Description |
|------------|-------------|
| **ETH1/ETH2/ETHSW** | Dual Gigabit Ethernet + Ethernet Switch |
| **DSIHOST/LTDC** | MIPI DSI Display + LCD Controller |
| **ES8388 (SAI1)** | Audio Codec (I2S interface) |
| **AT8563T (I2C)** | RTC Real-Time Clock |
| **CSI/DCMIPP** | Camera Serial Interface |
| **USB3DR/USBH_HS** | USB 3.0 Device + USB HS Host |
| **PCIE** | PCIe Interface |
| **SDMMC1/SDMMC2** | SD Card + eMMC Storage |
| **IPCC1** | Inter-Processor Communication |
| **FDCAN1/2/3** | CAN Bus |
| **UART4/USART1/2/UART7** | Multiple UART Interfaces |
| **PWM16** | LCD Backlight Control |
| **ADC1/2/3** | Analog-to-Digital Converters |
| **GPIO** | LED (Heartbeat), Buttons (User/Wake-up) |

## Directory Structure

```
STM32MP257DAK3/
├── CA35/                          # Cortex-A35 Application Processor
│   ├── manifest.prop              # OpenSTLinux release manifest
│   └── DeviceTree/STM32MP257DAK3/ # Device trees for all boot stages
│       ├── kernel/                # Linux kernel device tree
│       ├── optee-os/              # OP-TEE device tree
│       ├── tf-a/                  # TF-A device tree
│       └── u-boot/                # U-Boot device tree
├── CM33/                          # Cortex-M33 Coprocessor
│   ├── DeviceTree/STM32MP257DAK3/
│   │   └── tf-m/                  # TF-M device tree
│   └── NonSecure/                 # M33 NonSecure firmware
│       └── Core/
│           ├── Inc/               # Header files
│           ├── Src/               # Source files (main.c, interrupts, etc.)
│           └── Startup/           # ARM assembly startup file
├── Common/                        # Common system files
│   └── System/                    # M33 system initialization
├── Drivers/                       # STM32 HAL/CMSIS drivers
│   ├── CMSIS/                     # ARM CMSIS core support
│   └── STM32MP2xx_HAL_Driver/     # STM32 HAL hardware abstraction layer
├── STM32MP257DAK3.ioc             # STM32CubeMX project configuration
└── stm32mp257dak3.inc             # Yocto/OpenEmbedded machine configuration
```

## Getting Started

### Prerequisites

- STM32CubeMX (for peripheral configuration)
- STM32CubeIDE or ARM GCC toolchain (for M33 firmware)
- OpenSTLinux SDK (for A35 Linux development)
- Alientek STM32MP257 development board

### Build M33 Firmware

```bash
# Open project in STM32CubeIDE
# Build configuration: Debug/Release
# Target: Cortex-M33
```

### Flash and Boot

1. Build the complete boot chain (TF-A, OP-TEE, U-Boot, Linux kernel)
2. Flash images to SD card or eMMC
3. Boot the board

## Development History

| Date | Milestone |
|------|-----------|
| 2025-11-13 | Initial project commit |
| 2025-11-21 | DDR initialization successful |
| 2025-12-13 | U-Boot boot successful |
| 2025-12-23 | Linux Ethernet working |
| 2025-12-31 | MIPI DSI panel display working |
| 2026-01-09 | M33 coprocessor firmware added |
| 2026-01-20 | CSI camera interface support |
| 2026-03-02 | Audio (ES8388) and RTC (AT8563T) support |
| 2026-03-06 | Ethernet switch support |
| 2026-03-26 | Alientek 1+8G / 2+16G board support |

## License

This project uses STM32 HAL drivers and CMSIS libraries from STMicroelectronics.
See individual `License.md` files in the `Drivers/` directory for details.

## Links

- [STM32MP257 Product Page](https://www.st.com/en/microcontrollers-microprocessors/stm32mp257.html)
- [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)
- [OpenSTLinux](https://wiki.st.com/stm32mpu/wiki/OpenSTLinux_distribution)
- [Alientek (正点原子)](https://www.alientek.com/)
