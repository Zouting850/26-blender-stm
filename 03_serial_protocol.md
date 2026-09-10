# Serial Protocol

## 1. Scope

The updated protocol is centered on screen-side content switching instead of real-time Blender object control.

Two modes are supported:

- `DOCUMENT`
- `OBJECT`

## 2. Frame Format

Use readable ASCII frames:

```text
<SOURCE:COMMAND:VALUE>
```

Examples:

```text
<UI:MODE_DOCUMENT:1>
<UI:MODE_OBJECT:1>
<UI:SHOW_DOC_INTRO:1>
<UI:SHOW_DOC_BACKGROUND:1>
<UI:SHOW_DOC_YEAR:1>
<UI:SHOW_OBJ_INTRO:1>
<UI:SHOW_OBJ_BACKGROUND:1>
<UI:SHOW_OBJ_YEAR:1>
<UI:BACK:1>
<UI:HOME:1>
<MV:NEXT_PAGE:1>
<MV:PREV_PAGE:1>
```

## 3. TJC to STM32

Recommended TJC button messages:

```text
<UI:MODE_DOCUMENT:1>
<UI:MODE_OBJECT:1>
<UI:SHOW_DOC_INTRO:1>
<UI:SHOW_DOC_BACKGROUND:1>
<UI:SHOW_DOC_YEAR:1>
<UI:SHOW_OBJ_INTRO:1>
<UI:SHOW_OBJ_BACKGROUND:1>
<UI:SHOW_OBJ_YEAR:1>
<UI:PLAY_OBJECT:1>
<UI:REPLAY_OBJECT:1>
<UI:BACK:1>
<UI:HOME:1>
<UI:NEXT_PAGE:1>
<UI:PREV_PAGE:1>
```

## 4. OpenMV to STM32

OpenMV is only enabled in document mode.

Recommended messages:

```text
<MV:NEXT_PAGE:1>
<MV:PREV_PAGE:1>
```

STM32 behavior:

1. Parse the frame.
2. Check whether current mode is `DOCUMENT`.
3. If yes, trigger document page switching.
4. If current mode is `OBJECT`, ignore OpenMV page commands.

## 5. STM32 to TJC

STM32 may update status text on screen:

```text
t_mode.txt="DOCUMENT"
t_cmd.txt="PAGE NEXT"
t_link.txt="READY"
```

or:

```text
t_mode.txt="OBJECT"
t_cmd.txt="PLAY"
t_link.txt="READY"
```

All TJC commands still require trailing `FF FF FF`.
