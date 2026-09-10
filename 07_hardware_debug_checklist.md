# 硬件联调清单

## 1. 推荐联调顺序

```text
Blender 建模与渲染
-> TJC 页面与按钮事件
-> STM32 最小转发
-> TJC 与 STM32 状态联动
-> OpenMV 文献翻页手势
```

## 2. Blender 物品类检查

确认下面事项：

- 你的主模型或总父物体名称为 `RedRelic`
- 模型已经摆正朝向
- 材质和灯光已经可正常预览
- 可以运行 `D:\26嵌入式建模\blender\render_turntable_animation.py`

通过标准：

- 脚本运行后生成相机、灯光、旋转轨道
- 可以执行 `Render > Render Animation`

## 3. 文献翻页动画检查

确认下面事项：

- 可以运行 `D:\26嵌入式建模\blender\render_document_flip_overlay.py`
- 能生成 `20` 帧翻页覆盖动画
- 推荐切图时机记为第 `10` 帧

## 4. TJC 页面检查

确认：

- `page0 ~ page5` 已建立
- 文献页含 `p_doc`
- 文本组件名为 `t_mode / t_cmd / t_link`
- 按钮事件都使用 `<UI:命令:1>`

## 5. STM32 最小转发检查

接线：

```text
TJC -> USART1
STM32 USART2 -> USB-TTL -> 电脑串口助手
```

点击按钮后，电脑串口助手应收到：

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

## 6. TJC 页面联动检查

确认 STM32 上电后：

- 默认进入文献模式
- 自动显示 `page1`
- `p_doc.pic` 能对应当前页码切换
- 信息按钮可跳到 `page3 / page4 / page5`

## 7. OpenMV 文献翻页检查

仅在文献模式检查：

- 左右手势能稳定区分
- 串口输出只有：

```text
<MV:NEXT_PAGE:1>
<MV:PREV_PAGE:1>
```

- 切到物品模式后，STM32 应忽略这两个手势命令

## 8. 当前不要再做的旧链路

下面这条旧链路不再是主方案：

```text
TJC -> STM32 -> 电脑串口 -> Blender 实时缩放旋转
```

它可以保留作历史实验，但不作为最终交付结构。
