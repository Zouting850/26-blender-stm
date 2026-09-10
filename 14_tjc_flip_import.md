# TJC 文献翻页动画导入流程

## 1. 目标

把 Blender 生成的翻页覆盖动画导入 TJC，用在 `page1` 文献展示页上方。

## 2. 你最终会得到两组资源

来自：

`D:\26stm_blender\blender\document_flip_frames`

### 正向翻页

- `flip_0001.png`
- `flip_0002.png`
- ...
- `flip_0020.png`

### 反向翻页

- `flip_back_0001.png`
- `flip_back_0002.png`
- ...
- `flip_back_0020.png`

## 3. 在 Blender 里怎么出这两组图

### 第一步

运行：

`D:\26stm_blender\blender\render_document_flip_overlay.py`

### 第二步

在 Blender 顶部执行：

- `Render > Render Animation`

这样会先生成正向序列。

### 第三步

渲染完成后，在 Blender 的 Python Console 里执行：

```python
build_reverse_copy()
```

这样会自动生成反向序列。

## 4. 导入 TJC 的核心思路

文献页不是把整张文献做成视频，而是分成两层：

1. 底层：当前文献静态图片 `p_doc`
2. 上层：翻页覆盖动画

也就是：

- `p_doc` 一直显示真实文献内容
- 翻页时，局部播放 `flip_XXXX.png`
- 播到中间时切换 `p_doc.pic`
- 再继续播完后半段

## 5. 在 TJC 里怎么放

### 方法 A：如果你的 TJC 编辑器支持逐帧图片动画组件

直接：

- 导入 `flip_0001 ~ flip_0020`
- 新建一个覆盖在 `p_doc` 上方的动画组件
- 动画区域大小与文献显示区域一致
- 下一页时播放正向序列
- 上一页时播放反向序列

这是最理想的方式。

### 方法 B：如果没有现成动画组件

就用图片组件手动切换：

- 额外放一个覆盖层图片组件，例如 `p_flip`
- 用页面定时器或组件定时器按顺序改：
  - `p_flip.pic = flip_0001`
  - `p_flip.pic = flip_0002`
  - ...
  - `p_flip.pic = flip_0020`

同理，上一页时按 `flip_back_0001 ~ flip_back_0020` 切换。

## 6. 推荐切页时机

建议在第 `10` 帧附近切底图：

- `flip_0001 ~ flip_0009`：继续显示旧文献底图
- 第 `10` 帧：把 `p_doc.pic` 切到下一页
- `flip_0011 ~ flip_0020`：继续播放直到翻完

这样视觉最自然。

## 7. 资源导入顺序建议

### 文献底图

- `doc_page_01 ~ doc_page_08`
- 给 `p_doc` 使用

### 翻页覆盖图

- `flip_0001 ~ flip_0020`
- `flip_back_0001 ~ flip_back_0020`
- 给覆盖层组件使用

## 8. 你现在最务实的做法

先不要在 TJC 里一次做完整自动播放逻辑。

先分三步：

1. 先在 Blender 产出正向 20 帧
2. 导入 TJC，确认单张图能覆盖在 `p_doc` 上方
3. 再做逐帧切换逻辑

## 9. 当前建议的推进顺序

1. 先在 Blender 跑脚本
2. 执行 `Render Animation`
3. 执行 `build_reverse_copy()`
4. 检查 `document_flip_frames` 是否有两套 20 张图
5. 再把这 40 张图导入 TJC
