# Blender 资产制作清单

## 1. 当前目标拆分

分成两类资产：

1. 文献类翻页覆盖动画
2. 物品类 360 度展示动画

## 2. 文献类资产

### 已有脚本

`D:\26stm_blender\blender\render_document_flip_overlay.py`

### 你要完成的事

- 打开 Blender
- 运行脚本生成翻页场景
- 渲染 `20` 帧覆盖动画
- 输出到：

`D:\26stm_blender\blender\document_flip_frames`

### 交付目标

- `flip_01.png ~ flip_20.png`
- 最好再准备一套反向翻页序列，或确认 TJC 能否反向播放

## 3. 物品类资产

### 已有脚本

`D:\26stm_blender\blender\render_turntable_animation.py`

### 你要完成的事

- 手工完成文物建模
- 保证总父物体命名为 `RedRelic`
- 检查材质、灯光、构图
- 运行脚本生成环绕相机和关键帧
- 渲染 360 度展示动画

### 输出位置

`D:\26stm_blender\blender\turntable_frames`

## 4. 推荐制作顺序

1. 先完成文献底图
2. 再完成文献翻页覆盖动画
3. 然后完成物品类模型
4. 最后渲染物品类转台动画

## 5. 三天内最务实的推进方式

### 第一天

- 定稿 TJC 页面
- 跑通 `USB-TTL -> PA10` 所有命令
- 整理文献图片资源

### 第二天

- 生成文献翻页覆盖动画
- 导入 TJC 文献底图
- 完成文物模型主体

### 第三天

- 完成物品类灯光材质
- 渲染 360 度动画
- 等电平转换模块到货后补通 `TJC TX -> PA10`
