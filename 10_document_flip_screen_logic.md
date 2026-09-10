# Document Flip Screen Logic

## 1. Stable Display Strategy

Use a two-part document viewer inside the TJC screen:

1. static document base image
2. local page-flip cover animation

Do not rely on serial real-time frame streaming.

## 2. Why This Strategy

This is more stable because:

- STM32 only sends simple commands
- TJC plays local animation assets by itself
- OpenMV only decides `NEXT_PAGE` or `PREV_PAGE`
- there is no dependence on transparent PNG alpha blending

## 3. Viewport Layout

Inside the document page, split the UI into:

- top title area
- center document viewport
- bottom control area

The page-flip animation should cover only the center document viewport, not the full screen UI.

## 4. Recommended Asset Structure

Prepare:

- `doc_page_01.jpg`, `doc_page_02.jpg`, ...
- `flip_01.png` to `flip_20.png`
- optional page-turn sound effect

The `flip_XX.png` sequence is the generic cover animation generated from Blender.

## 5. Runtime Logic

### 5.1 Next Page

1. Current document page is visible as base image.
2. TJC or OpenMV triggers `NEXT_PAGE`.
3. STM32 sends one page-turn command to the screen logic.
4. Screen begins local playback of `flip_01` to `flip_20`.
5. When animation reaches the full-cover stage, switch base image to the next document page.
6. Animation continues until the next page is fully revealed.

### 5.2 Previous Page

Use one of these two methods:

- render a second reverse page-flip sequence
- or reuse the same sequence if screen-side reverse playback is supported

The safer choice is to prepare a dedicated reverse sequence.

## 6. Best Switching Moment

Recommended switch point:

- around frame `10`

Reason:

- at this point the page is near full cover
- switching the underlying document image is less noticeable

If precise frame callbacks are hard to implement on the screen, make frames `9` to `11` close to fully covered. This widens the safe switching window.

## 7. Blender Output Guidance

Use:

`D:\26嵌入式建模\blender\render_document_flip_overlay.py`

This script produces a fixed-size page-turn cover sequence for the document viewport.

Recommended production rule:

- use a generic pale paper page
- keep the background visually close to the TJC document viewport background
- prioritize stable cover and reveal, not perfect physical realism

Blender usage:

1. Open the scene prepared for document mode.
2. Make sure the viewport is in Object Mode.
3. Run `render_document_flip_overlay.py`.
4. Check that the script creates a `Flip_Page` overlay and a simple camera rig.
5. Use `Render > Render Animation` to export the frame sequence.

## 8. Button Set

Recommended document mode buttons:

- `Prev`
- `Next`
- `Intro`
- `Background`
- `Year`
- `Home`

## 9. OpenMV Boundary

OpenMV only sends:

```text
<MV:NEXT_PAGE:1>
<MV:PREV_PAGE:1>
```

It should not participate in:

- object rotation
- object zoom
- object playback control

This keeps the exhibition more reliable.
