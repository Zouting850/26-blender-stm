# USB-TTL 到 `PA10` 测试说明

## 1. 目的

在电平转换模块到货前，先用电脑模拟串口屏发命令，验证：

- `STM32 USART1_RX(PA10)` 正常
- `protocol.c` 命令解析正常
- `app_logic.c` 页面跳转逻辑正常

## 2. 接线

临时测试接法：

| USB-TTL | STM32F411RE |
|---|---|
| `TXD` | `PA10 / USART1_RX` |
| `GND` | `GND` |

保留原有这一路：

| STM32F411RE | TJC |
|---|---|
| `PA9 / USART1_TX` | `TJC RX` |
| `GND` | `TJC GND` |

注意：

- 这一步临时断开 `TJC TX -> PA10`
- 不要让 `TJC TX` 和 `USB-TTL TXD` 同时驱动 `PA10`

## 3. 推荐工具

可直接使用：

`D:\26stm_blender\pc\send_ui_commands.py`

安装依赖：

```text
pip install pyserial
```

## 4. 用法

### 4.1 交互模式

```text
python D:\26stm_blender\pc\send_ui_commands.py --port COM7
```

### 4.2 单次发送

```text
python D:\26stm_blender\pc\send_ui_commands.py --port COM7 --command MODE_DOCUMENT
python D:\26stm_blender\pc\send_ui_commands.py --port COM7 --command SHOW_DOC_INTRO
python D:\26stm_blender\pc\send_ui_commands.py --port COM7 --command BACK
python D:\26stm_blender\pc\send_ui_commands.py --port COM7 --command HOME
```

## 5. 推荐测试顺序

1. `MODE_DOCUMENT`
2. `SHOW_DOC_INTRO`
3. `BACK`
4. `SHOW_DOC_BACKGROUND`
5. `BACK`
6. `SHOW_DOC_YEAR`
7. `BACK`
8. `HOME`
9. `MODE_OBJECT`
10. `PLAY_OBJECT`
11. `SHOW_OBJ_INTRO`
12. `BACK`
13. `SHOW_OBJ_BACKGROUND`
14. `BACK`
15. `SHOW_OBJ_YEAR`
16. `BACK`
17. `HOME`

## 6. 通过标准

- `MODE_DOCUMENT` 后屏幕进入 `page1`
- `MODE_OBJECT` 后屏幕进入 `page2`
- `SHOW_DOC_*` 跳到 `page3 ~ page5`
- `SHOW_OBJ_*` 跳到 `page6 ~ page8`
- `BACK` 从文献信息页回 `page1`
- `BACK` 从物品信息页回 `page2`
- `HOME` 从任意页面回 `page0`
