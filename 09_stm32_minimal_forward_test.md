# STM32 最小联调测试

## 1. 测试目标

先验证下面这条链路已经打通：

```text
TJC 串口屏 -> STM32F411RE -> USB-TTL -> 电脑串口助手
```

这一阶段的目标不是控制 Blender，而是先确认：

- TJC 按钮事件发送正确
- STM32 能正确解析新协议
- STM32 能把识别到的命令转发到电脑串口

## 2. 当前工程路径

```text
D:\26嵌入式建模\stm32_f411re_tjc_pc
```

## 3. 当前串口分配

| 外设 | 引脚 | 用途 |
|---|---|---|
| `USART1` | `PA9 TX / PA10 RX` | 连接 TJC 串口屏 |
| `USART2` | `PA2 TX / PA3 RX` | 连接电脑 USB-TTL |
| `USART6` | 预留 | 后续可接 OpenMV |

## 4. 接线

### 4.1 TJC 到 STM32

| TJC | STM32F411RE |
|---|---|
| TX | `PA10 / USART1_RX` |
| RX | `PA9 / USART1_TX` |
| GND | GND |
| VCC | 5V |

### 4.2 USB-TTL 到 STM32

| USB-TTL | STM32F411RE |
|---|---|
| RXD | `PA2 / USART2_TX` |
| TXD | `PA3 / USART2_RX` |
| GND | GND |

## 5. 电脑串口助手参数

- 波特率：`115200`
- 数据位：`8`
- 停止位：`1`
- 校验位：`None`
- 显示方式：`ASCII`

## 6. 期望收到的转发命令

点击 TJC 页面按钮后，电脑串口助手应能看到类似：

```text
<MODE_DOCUMENT>
<MODE_OBJECT>
<SHOW_INTRO>
<SHOW_BACKGROUND>
<SHOW_YEAR>
<PLAY_OBJECT>
<REPLAY_OBJECT>
<BACK>
<HOME>
<NEXT_PAGE>
<PREV_PAGE>
```

## 7. 推荐测试顺序

### 7.1 模式切换

点击首页两个模式按钮，串口助手应看到：

```text
<MODE_DOCUMENT>
<MODE_OBJECT>
```

### 7.2 文献类页面

进入文献模式后，依次点击：

- 上一页
- 下一页
- 文物简介
- 历史背景
- 出土年份
- 返回首页

串口助手应依次收到：

```text
<PREV_PAGE>
<NEXT_PAGE>
<SHOW_INTRO>
<SHOW_BACKGROUND>
<SHOW_YEAR>
<HOME>
```

### 7.3 物品类页面

进入物品模式后，依次点击：

- 播放
- 重播
- 文物简介
- 历史背景
- 出土年份
- 返回首页

串口助手应依次收到：

```text
<PLAY_OBJECT>
<REPLAY_OBJECT>
<SHOW_INTRO>
<SHOW_BACKGROUND>
<SHOW_YEAR>
<HOME>
```

### 7.4 信息页测试

进入 `page3 ~ page8` 任意信息页后，点击“返回上一页”：

```text
<BACK>
```

预期行为：

- 文献信息页返回 `page1`
- 物品信息页返回 `page2`

## 8. 通过标准

满足下面 3 条即可认为 STM32 最小链路正确：

- TJC 每个按钮都能触发对应串口输出
- STM32 不再输出旧方案命令
- 串口输出与当前双模式设计完全一致

## 9. 常见错误

### 9.1 没有任何输出

优先检查：

- `USART1` 是否接到 TJC
- `USART2` 是否接到 USB-TTL
- 串口助手 COM 口是否选对
- TJC 按钮事件里是否真的写了 `print "<UI:...:1>"`

### 9.2 还能看到旧命令

如果还看到下面这些内容，说明工程还停留在旧版：

```text
<ZOOM_IN>
<ZOOM_OUT>
<ROTATE_LEFT>
<ROTATE_RIGHT>
<AUTO_SHOW>
<STOP>
```

这时应重新检查：

- `App\\protocol.c`
- `App\\app_logic.c`
- TJC 按钮事件内容

## 10. 下一步

这个测试通过后，再进入下一阶段：

1. 接入 TJC 页面资源与图片资源
2. 完成文献翻页动画联动
3. 接入 OpenMV 文献翻页手势
4. 导入物品类 360 度动画资源
