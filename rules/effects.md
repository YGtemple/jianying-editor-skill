---
name: effects
description: Searching for and applying effects, filters, transitions, masks, mix modes, and chroma key.
metadata:
  tags: effects, filters, transitions, search, asset_id, masks, chroma, mix-modes, beauty
---

# Effects, Filters & Transitions

**NEVER guess asset IDs.** Always search first with `asset_search.py`.

```bash
python <SKILL_ROOT>/scripts/asset_search.py "复古" -c filters
python <SKILL_ROOT>/scripts/asset_search.py "雾化" -c transitions
```

Categories: `filters`, `video_scene_effects`, `transitions`, `text_animations`.

## VFX API

```python
# Scene effect
project.add_effect_simple(effect_name="故障_I", start_time="0s", duration="3s")

# Transition between clips
project.add_transition_simple(transition_name="叠化", video_segment=my_seg, duration="0.5s")
```

## Transition Reference (80+ total)

| Name | resource_id | Duration |
|------|-------------|----------|
| 叠化 | 6724845717472416269 | 0.5s |
| 淡入 | 6724845717472416269 | 0.5s |
| 左移 | 6724846395116753416 | 0.5s |
| 右移 | 6726711296063967748 | 1.0s |

Full list in `data/transitions.csv`.

## Mask Types (6)

| Type | resource_id |
|------|-------------|
| 线性 | 6791652175668843016 |
| 镜面 | 6791699060140020232 |
| 圆形 | 6791700663249146381 |
| 矩形 | 6791700809454195207 |
| 爱心 | 6794051276482023949 |
| 星形 | 6794051169434997255 |

## Mix Modes (10)

正片叠底, 颜色减淡, 颜色加深, 线性加深, 柔光, 强光, 滤色, 叠加, 变亮, 变暗

## Chroma Key (绿幕)

```jsonc
{
  "type": "chroma",
  "color": "#00FF00FF",
  "intensity_value": 0.2,
  "spill_value": 0.0
}
```

## Filters (300+)

Full list in `data/filters.csv`. Notable: 1980, ABG, 哈苏蓝, 书意, 冷白, 冰肌.

---

## Source

- pyJianYingDraft: effect_segment.py, track.py
- capcut-mate: transition_meta.py, filter_meta.py
- capcut-cli: mix modes, chroma key, mask configs