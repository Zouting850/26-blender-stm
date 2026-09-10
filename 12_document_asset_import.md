# 文献图片资源导入规范

## 1. 目标

把文献页图片整理成可直接给 `p_doc.pic` 调用的资源顺序。

## 2. 当前代码约束

`D:\26stm_blender\stm32_f411re_tjc_pc\App\app_logic.c`

当前逻辑固定为：

- `0 ~ 8` 已经被各个 `page` 的底图占用
- 文献第一页 -> `p_doc.pic = 9`
- 文献第二页 -> `p_doc.pic = 10`
- 文献第三页 -> `p_doc.pic = 11`

也就是：

```text
图片资源编号 = 9 + 文献页码 - 1
```

## 3. 建议文件命名

在电脑上先整理成：

```text
doc_page_01.jpg
doc_page_02.jpg
doc_page_03.jpg
```

## 4. 图片制作建议

- 比例尽量接近 `p_doc` 实际显示区域
- 保持同一裁切边距
- 对比度适中，优先保证文字可读
- 尽量统一亮度和底色

## 5. 导入 TJC 时的要求

导入顺序必须连续，且前面先保留 `0 ~ 8` 作为各页面底图：

| TJC 图片编号 | 对应内容 |
|---|---|
| `0 ~ 8` | 各个 `page` 的底图 |
| `9` | `doc_page_01` |
| `10` | `doc_page_02` |
| `11` | `doc_page_03` |

不要在 `9 ~ 11` 中间插入其他无关图片，否则 `p_doc.pic` 编号会错位。

## 6. 推荐联调方式

在电平转换模块到货前，先用：

`D:\26stm_blender\docs\11_usbttl_pa10_test.md`

里的方式验证 `NEXT_PAGE / PREV_PAGE` 是否能正确切页。

## 7. 后续叠加翻页动画

文献底图导入完成后，再叠加：

- `flip_01.png ~ flip_20.png`
- 可选翻页音效

详细逻辑见：

`D:\26stm_blender\docs\10_document_flip_screen_logic.md`

