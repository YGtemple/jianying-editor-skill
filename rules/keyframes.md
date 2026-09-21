---
name: keyframes
description: Adding keyframe animations (Zoom, Position, Opacity, Rotation, Color, Volume) to media segments.
metadata:
  tags: keyframes, animation, pip, zoom, pan, color, volume, easing, ken-burns
---

# Keyframes & Animation

```python
from pyJianYingDraft import KeyframeProperty as KP

segment = project.add_media_safe(r"C:\assets\image.png", start_time=1000000, duration="4s")
if segment:
    segment.add_keyframe(KP.uniform_scale, 1000000, 1.0)
    segment.add_keyframe(KP.uniform_scale, 5000000, 1.5)
```

## Supported Properties

| Python | JSON property_type | Range |
|--------|-------------------|-------|
| `KP.position_x` | `KFTypePositionX` | -1.0 to 1.0 |
| `KP.position_y` | `KFTypePositionY` | -1.0 to 1.0 |
| `KP.rotation` | `KFTypeRotation` | degrees |
| `KP.scale_x/y` | `KFTypeScaleX/Y` | multiplier |
| `KP.uniform_scale` | `UNIFORM_SCALE` | multiplier |
| `KP.alpha` | `KFTypeAlpha` | 0-1 |
| `KP.saturation` | `KFTypeSaturation` | -1 to 1 |
| `KP.contrast` | `KFTypeContrast` | -1 to 1 |
| `KP.brightness` | `KFTypeBrightness` | -1 to 1 |
| `KP.volume` | `KFTypeVolume` | 0-1 (audio only) |

## Critical Gotchas

1. **UNIFORM_SCALE** uses bare string `UNIFORM_SCALE`, NOT `KFTypeUniformScale`
2. **Alpha keyframes may not render** on video segments - use intro/outro animations instead
3. **Time is microseconds** (1 second = 1,000,000 us)
4. **time_offset is relative to segment start** (not timeline origin)

## Interpolation

- `"Line"` = linear, `"Smooth"` = ease in/out, `"Beizer"` = custom, `"FreeCurveInOut"` = bezier handles

## Examples

- **Ken Burns**: position_x 0->0.3, uniform_scale 1.0->1.2 over 8s
- **Volume ducking**: volume 1.0->0.2 at 5s, restore at 20s
- **Color grading**: brightness 0->0.3 over 10s

---

## Source

- pyJianYingDraft: keyframe.py, segment.py
- capcut-cli: UNIFORM_SCALE naming gotcha, alpha render trap
- pyCapCut: time_offset semantics, speed material