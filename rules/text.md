---
name: text-subtitles
description: Rules for adding text, subtitles, captions, and styled text (花字/Flower Text), including animations, fonts, borders, shadows, and bubbles.
metadata:
  tags: text, subtitles, captions, font, styled-text, flower-text, animation, border, shadow, bubble, srt
---

# Text & Subtitles

Use `add_text_simple()` for plain text, `add_styled_text()` for styled flower text (花字), or `import_srt()` for SRT subtitles.

## 1. Plain Text (普通文字)

```python
project.add_text_simple(
    text="Hello World",
    start_time="0s",
    duration="3s",
    clip_settings=draft.ClipSettings(transform_y=-0.8),
    font_size=12.0,
    color_rgb=(1, 1, 1),
    anim_in=None
)
```

### Constraints
- **Vertical Position (`clip_settings.transform_y`)**: `-0.8` is standard for subtitles. `0.0` is centered. `0.8` is for titles.
- **Duration**: MUST be specified explicitly.

## 2. Styled Text / Flower Text (花字)

Use `add_styled_text()` for decorative/styled text. See `data/cloud_text_styles.csv` for available styles.

```python
project.add_styled_text(
    text="This is styled!",
    style_id="7351316503771368713",
    start_time="5s",
    duration="3s",
    transform_y=-0.8
)
```

## 3. TextStyle Parameters

| Parameter | Type | Default | Notes |
|-----------|------|---------|-------|
| `size` | float | 5.0-8.0 | Font size scale |
| `bold` | bool | False | |
| `color` | tuple | (1,1,1) | RGB 0-1 |
| `alpha` | float | 1.0 | Opacity |
| `align` | int | 0 | 0=left,1=center,2=right |
| `letter_spacing` | int | 0 | value x 0.05 |
| `line_spacing` | int | 0 | 0.02 + value x 0.05 |

## 4. TextBorder / TextShadow

```python
border = draft.TextBorder(color=(0,0,0), alpha=1.0, width=40.0)
shadow = draft.TextShadow(color=(0,0,0), alpha=1.0, diffuse=15.0, distance=5.0, angle=-45.0)
```

## 5. Text Animations

- **in** (入场), **out** (出场), **loop** (循环)
- 60+ intro animations, 20+ outro, 25+ loop
- Use `anim_in`, `anim_out`, `anim_loop` kwargs
- Full lists in `data/text_animations.csv`

## 6. SRT Import

```python
project.import_srt(r"C:\path\to\subs.srt", track_name="Subtitles")
```

---

## Source

- pyJianYingDraft: text_intro.py, text_outro.py, text_loop.py, font_meta.py
- capcut-mate: huazi.json (~200+ 花字 definitions)
- capcut-cli: double-serialization, UTF-16 range offsets