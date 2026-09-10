# TJC 与 STM32 联调

## 1. 当前目标

验证这条链路：

```text
TJC 按钮事件 -> STM32F411RE -> 串口解析 -> 页面/状态联动
```

如果同时接了 `USB-TTL`，还可以顺带验证电脑串口输出是否正确。

## 2. TJC 按钮事件格式

每个按钮的弹起事件中写一条 `print`：

```text
print "<UI:MODE_DOCUMENT:1>"
print "<UI:MODE_OBJECT:1>"
print "<UI:SHOW_INTRO:1>"
print "<UI:SHOW_BACKGROUND:1>"
print "<UI:SHOW_YEAR:1>"
print "<UI:PLAY_OBJECT:1>"
print "<UI:REPLAY_OBJECT:1>"
print "<UI:BACK:1>"
print "<UI:HOME:1>"
print "<UI:NEXT_PAGE:1>"
print "<UI:PREV_PAGE:1>"
```

一个按钮只保留对应自己功能的那一行。

## 3. 接线

### 3.1 TJC 到 USART1

| TJC | STM32F411RE |
|---|---|
| TX | `PA10 / USART1_RX` |
| RX | `PA9 / USART1_TX` |
| GND | GND |
| VCC | 5V |

### 3.2 USB-TTL 到 USART2

| STM32F411RE | USB-TTL |
|---|---|
| `PA2 / USART2_TX` | RXD |
| `PA3 / USART2_RX` | TXD |
| GND | GND |

## 4. 串口助手参数

```text
115200 8N1 ASCII
```

## 5. 预期调试现象

### 5.1 模式按钮

点击首页按钮后，应看到：

| 按钮 | 输出 |
|---|---|
| 文献类 | `<MODE_DOCUMENT>` |
| 物品类 | `<MODE_OBJECT>` |

### 5.2 文献类按钮

| 按钮 | 输出 |
|---|---|
| 上一页 | `<PREV_PAGE>` |
| 下一页 | `<NEXT_PAGE>` |
| 文物简介 | `<SHOW_INTRO>` |
| 历史背景 | `<SHOW_BACKGROUND>` |
| 出土年份 | `<SHOW_YEAR>` |
| 返回首页 | `<HOME>` |

### 5.3 物品类按钮

| 按钮 | 输出 |
|---|---|
| 播放 | `<PLAY_OBJECT>` |
| 重播 | `<REPLAY_OBJECT>` |
| 文物简介 | `<SHOW_INTRO>` |
| 历史背景 | `<SHOW_BACKGROUND>` |
| 出土年份 | `<SHOW_YEAR>` |
| 返回首页 | `<HOME>` |

### 5.4 信息页按钮

| 按钮 | 输出 |
|---|---|
| 返回上一页 | `<BACK>` |

## 6. STM32 当前默认屏幕状态

`D:\26嵌入式建模\stm32_f411re_tjc_pc\App\app_logic.c` 当前默认：

- 上电进入 `DOCUMENT`
- 默认显示 `page1`
- 文献页图像组件为 `p_doc`
- 状态文本为 `t_mode / t_cmd / t_link`

## 7. 常见错误

### 7.1 没反应

检查：

- TJC 按钮事件是否真的下载到了屏幕
- `USART1` 线是否接反
- 屏幕和 STM32 是否共地

### 7.2 收到旧命令

如果串口助手还显示：

```text
<ZOOM_IN>
<ROTATE_LEFT>
<AUTO_SHOW>
```

说明 TJC 工程或 STM32 工程仍有旧版命令残留。

### 7.3 页面切换不对

检查：

- 页面编号是否仍为 `page0 ~ page5`
- 组件名是否仍为 `p_doc / t_mode / t_cmd / t_link`
