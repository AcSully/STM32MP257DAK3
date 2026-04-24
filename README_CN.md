# STM32MP257DAK3 板级支持包

这是一个基于 STM32CubeMX 生成的 **STM32MP257DAK3** 微处理器 BSP（板级支持包），面向**正点原子 STM32MP257 开发板**。

## 支持的板卡

| 板卡 | 配置 | 状态 |
|------|------|------|
| 正点原子 STM32MP257 | 1+8G（1GB DDR + 8GB eMMC） | ✅ 已支持 |
| 正点原子 STM32MP257 | 2+16G（2GB DDR + 16GB eMMC） | ✅ 已支持 |

## 硬件概述

**SoC**: STM32MP257DAK3（STM32MP2 系列）
- **应用处理器**: 双核 Cortex-A35，主频最高 1.5GHz
- **协处理器**: Cortex-M33，主频 400MHz
- **GPU**: Vivante GC7000UL（3D/2D 图形加速）
- **视频**: 硬件 H.264/H.264 编解码器

## 软件架构

```
┌─────────────────────────────────────────────────────────────┐
│                    Cortex-A35 (CA35) 应用处理器              │
│   TF-A → OP-TEE → U-Boot → Linux (OpenSTLinux 6.6)         │
│   设备树: kernel / u-boot / tf-a / optee-os                 │
├─────────────────────────────────────────────────────────────┤
│              IPCC1（核间通信）                               │
│                    OpenAMP / RPMSG                          │
├─────────────────────────────────────────────────────────────┤
│                    Cortex-M33 (CM33) 协处理器               │
│   STM32 HAL 驱动 + IPCC/UART4（非安全固件）                 │
│   TF-M（可信固件-M）                                        │
└─────────────────────────────────────────────────────────────┘
```

**OpenSTLinux 版本**: `openstlinux-6.6-yocto-scarthgap-mpu-v25.06.11`

## 支持的外设

| 外设 | 说明 |
|------|------|
| **ETH1/ETH2/ETHSW** | 双千兆以太网 + 以太网交换机 |
| **DSIHOST/LTDC** | MIPI DSI 显示接口 + LCD 控制器 |
| **ES8388 (SAI1)** | 音频编解码器（I2S 接口） |
| **AT8563T (I2C)** | RTC 实时时钟 |
| **CSI/DCMIPP** | 摄像头串行接口 |
| **USB3DR/USBH_HS** | USB 3.0 设备 + USB HS 主机 |
| **PCIE** | PCIe 接口 |
| **SDMMC1/SDMMC2** | SD 卡 + eMMC 存储 |
| **IPCC1** | 核间通信 |
| **FDCAN1/2/3** | CAN 总线 |
| **UART4/USART1/2/UART7** | 多路串口 |
| **PWM16** | LCD 背光控制 |
| **ADC1/2/3** | 模数转换器 |
| **GPIO** | LED（心跳灯）、按键（User/Wake-up） |

## 目录结构

```
STM32MP257DAK3/
├── CA35/                          # Cortex-A35 应用处理器
│   ├── manifest.prop              # OpenSTLinux 发行版清单
│   └── DeviceTree/STM32MP257DAK3/ # 各启动阶段的设备树
│       ├── kernel/                # Linux 内核设备树
│       ├── optee-os/              # OP-TEE 设备树
│       ├── tf-a/                  # TF-A 设备树
│       └── u-boot/                # U-Boot 设备树
├── CM33/                          # Cortex-M33 协处理器
│   ├── DeviceTree/STM32MP257DAK3/
│   │   └── tf-m/                  # TF-M 设备树
│   └── NonSecure/                 # M33 非安全固件
│       └── Core/
│           ├── Inc/               # 头文件
│           ├── Src/               # 源代码（main.c、中断等）
│           └── Startup/           # ARM 汇编启动文件
├── Common/                        # 公共系统文件
│   └── System/                    # M33 系统初始化
├── Drivers/                       # STM32 HAL/CMSIS 驱动库
│   ├── CMSIS/                     # ARM CMSIS 核心支持
│   └── STM32MP2xx_HAL_Driver/     # STM32 HAL 硬件抽象层
├── STM32MP257DAK3.ioc             # STM32CubeMX 工程配置文件
└── stm32mp257dak3.inc             # Yocto/OpenEmbedded 机器配置
```

## 快速开始

### 环境准备

- STM32CubeMX（用于外设配置）
- STM32CubeIDE 或 ARM GCC 工具链（用于 M33 固件开发）
- OpenSTLinux SDK（用于 A35 Linux 开发）
- 正点原子 STM32MP257 开发板

### 编译 M33 固件

```bash
# 在 STM32CubeIDE 中打开工程
# 编译配置: Debug/Release
# 目标内核: Cortex-M33
```

### 烧录与启动

1. 编译完整的启动链（TF-A、OP-TEE、U-Boot、Linux 内核）
2. 将镜像烧录到 SD 卡或 eMMC
3. 启动开发板

## 开发历程

| 日期 | 里程碑 |
|------|--------|
| 2025-11-13 | 首次提交 |
| 2025-11-21 | DDR 初始化成功 |
| 2025-12-13 | U-Boot 启动成功 |
| 2025-12-23 | Linux 以太网调通 |
| 2025-12-31 | MIPI DSI 显示屏显示正常 |
| 2026-01-09 | M33 协处理器固件添加 |
| 2026-01-20 | CSI 摄像头接口支持 |
| 2026-03-02 | 音频（ES8388）和 RTC（AT8563T）支持 |
| 2026-03-06 | 以太网交换机支持 |
| 2026-03-26 | 正点原子 1+8G / 2+16G 板卡适配 |

## 许可证

本项目使用了 STMicroelectronics 的 STM32 HAL 驱动和 CMSIS 库。
请查看 `Drivers/` 目录下的 `License.md` 文件了解详细信息。

## 相关链接

- [STM32MP257 产品页面](https://www.st.com/en/microcontrollers-microprocessors/stm32mp257.html)
- [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)
- [OpenSTLinux](https://wiki.st.com/stm32mpu/wiki/OpenSTLinux_distribution)
- [正点原子官网](https://www.alientek.com/)
