# 硬件接线说明

## 1. 当前硬件

| 模块 | 型号/说明 |
|---|---|
| 主控 | `STM32F411RE` |
| 串口屏 | `TJC1060X570_011C_I` |
| 视觉模块 | `OpenMV3 R2` |
| 调试串口 | `USB-TTL` |
| 建模渲染 | 电脑上的 `Blender` |

## 2. 当前系统角色

| 模块 | 角色 |
|---|---|
| `TJC` | 最终展示终端、按钮交互终端 |
| `STM32` | 协议解析、模式切换、状态转发 |
| `OpenMV` | 仅文献类翻页手势 |
| `Blender` | 离线建模、材质灯光、360 度动画渲染 |

## 3. STM32 串口分配

| 串口 | 引脚 | 连接对象 | 用途 | 波特率 |
|---|---|---|---|---|
| `USART1` | `PA9 TX / PA10 RX` | `TJC` | 接收按钮事件、回写界面状态 | `115200` |
| `USART2` | `PA2 TX / PA3 RX` | `USB-TTL` | 输出调试命令到电脑 | `115200` |
| `USART6` | 预留 | `OpenMV` | 文献类手势输入 | `115200` |

## 4. TJC 到 STM32

| TJC | STM32F411RE |
|---|---|
| TX | `PA10 / USART1_RX` |
| RX | `PA9 / USART1_TX` |
| GND | GND |
| VCC | 5V |

如果你的屏幕 TX 是 5V TTL，建议确认是否需要做电平兼容。

## 5. USB-TTL 到 STM32

| USB-TTL | STM32F411RE |
|---|---|
| RXD | `PA2 / USART2_TX` |
| TXD | `PA3 / USART2_RX` |
| GND | GND |

## 6. OpenMV 到 STM32

当前代码默认还没有在 MCU 侧启用 OpenMV。

后续接入时建议：

| OpenMV | STM32F411RE |
|---|---|
| TX | `USART6_RX` |
| RX | `USART6_TX` |
| GND | GND |
| VCC | 5V / 按模块要求 |

## 7. 当前最小联调链路

建议先跑通这条最小链路：

```text
TJC -> STM32 USART1 -> STM32 USART2 -> USB-TTL -> 电脑串口助手
```

等这条链路稳定后，再接入：

```text
OpenMV -> STM32 -> TJC
```

## 8. Blender 在当前方案中的位置

Blender 不再作为实时串口控制终端。

现在它只负责两件事：

1. 物品类红色文物建模
2. 导出 360 度展示动画或序列帧
