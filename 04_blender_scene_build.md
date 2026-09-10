# Blender Object Relic Build Guide

## 1. Goal

Object relic mode no longer depends on real-time serial control inside Blender.

The new Blender task is:

1. build the relic model
2. set materials
3. set lighting
4. create a 360-degree turntable shot
5. render the animation for TJC screen playback

## 2. Object Structure

Recommended hierarchy:

| Object Name | Purpose |
|---|---|
| `RedRelic` | main root empty or parent object |
| `Relic_Main` | main relic body |
| `Relic_Base` | stand / support / plinth |
| `Title_Text` | optional display title |

Keep the final parent object named `RedRelic` so the existing project assets remain compatible.

## 3. Materials

Suggested approach:

- use one main material for the relic body
- use one darker material for grooves, edges, or supports
- keep roughness moderate so the object reads clearly on screen
- avoid over-reflective surfaces unless the relic is metallic

## 4. Lighting

Recommended three-light setup:

- key light at front-top
- fill light at side
- back light to separate silhouette from background

Background should be darker than the relic so the 360-degree rotation stays readable on the serial screen.

## 5. Camera

Use a fixed target and rotate the camera rig around the relic for one full circle.

Suggested values:

- focal length: `35mm` to `50mm`
- total rotation: `360`
- frame count: `180` or `240`
- output ratio: close to the TJC screen aspect

## 6. Automation Script

Use:

`D:\26嵌入式建模\blender\render_turntable_animation.py`

This script:

- finds `RedRelic`
- creates a turntable camera rig
- sets render output to `1060 x 570`
- inserts linear 360-degree rotation animation

After running it in Blender, use `Render > Render Animation`.

## 7. Final Export Idea

The rendered result can be:

- image sequence
- video converted from image sequence

The TJC side only needs the final visual playback asset. The object mode does not require OpenMV interaction.
