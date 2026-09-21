---
name: jianying-editor
description: 剪映 (JianYing / CapCut) 终极自动化剪辑技能 — 桌面自动化 + 草稿 JSON 操作 + 批量无头导出 + 说话人智能剪辑 (talking head / 静音去除) + 字幕生成 + 滤镜特效转场 + TTS 配音 + BGM 配乐 + 屏幕录制 + Web 动效合成 (HTML/JS/Canvas → 透明视频)。支持录屏、素材导入、字幕生成、Web 动效合成、项目导出，以及整合自 9 个开源社区项目的草稿结构知识。
version: 2.0.0
metadata:
  openclaw:
    requires:
      env:
        - JY_SKILL_ROOT
      bins:
        - python3
        - ffmpeg
    primaryEnv: JY_SKILL_ROOT
    emoji: "🎬"
    homepage: https://github.com/isYangs/jianying-editor-skill
---

# JianYing Editor Skill — Ultimate Automation Router

Use this skill when the user wants to **automate video editing, generate drafts, manipulate media assets, run batch/headless export, apply talking-head smart cuts, generate subtitles, compose TTS + BGM, record screen, or render web VFX** in JianYing Pro / CapCut.

This is the **single entry point** for the entire integrated skill. It routes every possible editing scenario to the right rule file, example, or script. Read the routing table below, then open the referenced rule file.

> **Knowledge base**: This skill integrates draft-JSON knowledge extracted from **9 open-source community projects** (capcut-mate, pyJianYingDraft, pyCapCut, VectCutAPI, jianying-mcp, capcut-mcp-server, capcut-mcp, capcut-ai-editor/SmartCut, capcut-cli). See [references/community-tools.md](references/community-tools.md) for the full comparison and when to lean on an external project.

---

## Agent execution playbook

- Full playbook: [docs/agent-playbook.md](docs/agent-playbook.md)
- Minimal command SOP: [docs/minimal-command-sop.md](docs/minimal-command-sop.md)
- Draft inspector CLI:

```bash
python <SKILL_ROOT>/scripts/draft_inspector.py list --limit 20
python <SKILL_ROOT>/scripts/draft_inspector.py summary --name "DraftName"
python <SKILL_ROOT>/scripts/draft_inspector.py show --name "DraftName" --kind content --json
```

For generic editing requests, always follow the "Quick Edit Runtime Template" and "Acceptance Checklist" in that playbook.

---

## 🚨 重要开发原则 (CRITICAL DEVELOPER RULES)

1. **脚本位置**：**禁止在 Skill 内部目录创建剪辑脚本**。所有的剪辑逻辑实现代码（`.py` 脚本）必须存放在用户当前项目的**根目录**（或子目录，如 `scripts/`），以保持 Skill 库的纯净和可移植性。
2. **配乐选择**：
   - **简单演示使用默认音乐**。实际项目，应优先检索并推荐 `data/cloud_music_library.csv` 中的相关曲目，或根据视频主题（如"科技"、"温暖"）进行关键词过滤。
   - 询问用户："我发现了几首符合主题的云端音乐，要不要试试？（如：`Illuminate` - 科技感）"。
3. **项目分辨率**：初始化 `JyProject` 时必须按主素材比例设置分辨率。**默认横屏 1920×1080**；竖屏必须显式 `width=1080, height=1920`。
4. **保存**：脚本末尾**必须**调用 `project.save()`。
5. **不猜测资源 ID**：剪映资源 ID（滤镜/转场/特效/动画）随版本变化，**必须**先用 `scripts/asset_search.py` 搜索。

---

## 📚 Rule Files (Read these for specific tasks)

| Rule file | What it covers |
|---|---|
| [rules/setup.md](rules/setup.md) | **Mandatory** Python bootstrap / `sys.path` resolution for every script |
| [rules/core.md](rules/core.md) | Core `JyProject` ops: project creation, `save()`, template clone, auto-export, acceptance checklist |
| [rules/cli.md](rules/cli.md) | CLI contracts: draft inspector, diagnostics, asset search, export, `--json` output contract |
| [rules/media.md](rules/media.md) | Importing video/audio/image assets, cloud media, AI video analysis optimization (30m/360p), ratio rules |
| [rules/text.md](rules/text.md) | Plain text, flower text (花字), SRT import, auto-layering, subtitle positioning |
| [rules/keyframes.md](rules/keyframes.md) | Keyframe animations (zoom, pan, opacity, rotation) via `KeyframeProperty` |
| [rules/effects.md](rules/effects.md) | Searching/applying filters, transitions, scene effects, character effects |
| [rules/recording.md](rules/recording.md) | Screen recording (Windows/macOS) + smart-zoom keyframe auto-generation |
| [rules/web-vfx.md](rules/web-vfx.md) | Web-to-Video VFX: HTML/JS/Canvas → transparent MP4 via Playwright |
| [rules/generative.md](rules/generative.md) | Chain-of-thought for generative/AI editing; movie commentary builder |
| [rules/audio-voice.md](rules/audio-voice.md) | TTS voiceover, narrated subtitles, BGM/SFX sourcing, mixing rules |
| [rules/draft-structure.md](rules/draft-structure.md) | **NEW** — Deep dive into `draft_content.json` schema: tracks, materials, segments, mirror files, JianYing 6.0+ encryption |
| [rules/batch-export.md](rules/batch-export.md) | **NEW** — Headless/batch export: auto-deploy, desktop hot-reload, OSS upload, ffmpeg proxy render, batch draft update |
| [rules/talking-head.md](rules/talking-head.md) | **NEW** — Talking-head smart cut: silence removal via subtitle gaps, duplicate-take detection, in-place draft editing |

---

## 🎯 Agent Quick Routing Table

| Scenario | Go to rule(s) | Example / tool |
|---|---|---|
| Basic clip creation / cut / assemble | `rules/core.md` + `rules/media.md` | `examples/simple_clip_demo.py`, `examples/my_first_vlog.py` |
| Subtitles / text / flower text (花字) / SRT | `rules/text.md` | `examples/cloud_video_music_tts_demo.py` |
| Keyframes / animations / Ken Burns / PIP zoom | `rules/keyframes.md` | capture segment from `add_media_safe`, then `.add_keyframe()` |
| Effects / filters / transitions / masks | `rules/effects.md` | `scripts/asset_search.py "<kw>" -c filters` |
| TTS voiceover / narrated subtitles / BGM / SFX | `rules/audio-voice.md` | `examples/cloud_video_music_tts_demo.py` |
| Screen recording + smart zoom | `rules/recording.md` | `tools/recording/recorder.py` (Win) / `tools/recording/macos_recorder.py` (mac) |
| Web VFX (data viz, 3D, glassmorphic overlays) | `rules/web-vfx.md` | `examples/web_to_video_intro_demo.py` |
| Generative / AI editing / movie commentary | `rules/generative.md` | `scripts/movie_commentary_builder.py` |
| Draft JSON deep dive (schema, mirrors, encryption) | `rules/draft-structure.md` + `references/draft-json-schema.md` | `scripts/draft_inspector.py show --kind content --json` |
| Batch / headless export / auto-deploy | `rules/batch-export.md` + `rules/core.md` | `examples/robust_auto_export.py`, `scripts/auto_exporter.py` |
| Talking head / silence removal / repeated takes | `rules/talking-head.md` | SmartCut-style gap analysis on subtitle track |
| CLI diagnostics / environment check | `rules/cli.md` | `scripts/api_validator.py`, `scripts/draft_inspector.py` |
| Setup / bootstrap / import path | `rules/setup.md` | Quick-start template below |
| Community tools reference (9 projects) | `references/community-tools.md` | choose right external library |
| capcut-mate HTTP API reference | `references/capcut-mate-api.md` | 39 REST endpoints |
| Draft JSON schema reference | `references/draft-json-schema.md` | full field map |

### Named workflows

- **Cloud video + cloud music + TTS**: `rules/media.md` + `rules/audio-voice.md` → `examples/cloud_video_music_tts_demo.py`
- **Narration + subtitle alignment**: `rules/text.md` + `rules/audio-voice.md` → `examples/cloud_video_music_tts_demo.py`
- **Screen recording + smart zoom**: `rules/recording.md` → `tools/recording/recorder.py` / `tools/recording/macos_recorder.py`
- **Batch/headless export**: `rules/batch-export.md` + `rules/core.md` → `examples/robust_auto_export.py`
- **Movie commentary generation**: `rules/generative.md` → `scripts/movie_commentary_builder.py`
- **Compound clip (nested project)**: `examples/compound_clip_demo.py`
- **CV-assisted exposure alignment**: `examples/auto_exposure_align_demo.py`
- **AI transcribe → match B-roll → assemble**: `examples/video_transcribe_and_match.py`

---

## 📖 Examples

Refer to these for complete workflows:

- [examples/my_first_vlog.py](examples/my_first_vlog.py) — Complete vlog with BGM and animated text
- [examples/simple_clip_demo.py](examples/simple_clip_demo.py) — Quick-start tutorial for basic cutting and track management
- [examples/compound_clip_demo.py](examples/compound_clip_demo.py) — Professional nested project (Compound Clip) automation
- [examples/cloud_video_music_tts_demo.py](examples/cloud_video_music_tts_demo.py) — Cloud video + cloud BGM + TTS/subtitle alignment
- [examples/web_to_video_intro_demo.py](examples/web_to_video_intro_demo.py) — Web-to-Video intro (HTML animation → timeline clip)
- [examples/robust_auto_export.py](examples/robust_auto_export.py) — Stable export workflow and failure handling
- [examples/auto_exposure_align_demo.py](examples/auto_exposure_align_demo.py) — CV-assisted exposure alignment
- [examples/video_transcribe_and_match.py](examples/video_transcribe_and_match.py) — **Advanced**: AI-driven transcribe → B-roll match → assemble
- [examples/all_elements_regen.py](examples/all_elements_regen.py) — Regenerate all elements
- [examples/macos_demo.py](examples/macos_demo.py) — macOS-specific demo

---

## 🛠️ Scripts & Tools

### Asset search

```bash
python <SKILL_ROOT>/scripts/asset_search.py "复古" -c filters
python <SKILL_ROOT>/scripts/asset_search.py "雾化" -c transitions
```

Categories: `filters`, `video_scene_effects`, `transitions`, `text_animations`.

### Draft inspector

```bash
python <SKILL_ROOT>/scripts/draft_inspector.py list --limit 20
python <SKILL_ROOT>/scripts/draft_inspector.py summary --name "DraftName"
python <SKILL_ROOT>/scripts/draft_inspector.py show --name "DraftName" --kind content --json
```

### Diagnostics

```bash
python <SKILL_ROOT>/scripts/api_validator.py --json
python <SKILL_ROOT>/scripts/diag_drafts.py
python <SKILL_ROOT>/scripts/diag_window.py
python <SKILL_ROOT>/scripts/diag_desc.py
```

### Export

```bash
# Headless export to MP4
python <SKILL_ROOT>/scripts/auto_exporter.py "DraftName" "output.mp4" --res 1080 --fps 60

# SRT only
python <SKILL_ROOT>/scripts/jy_wrapper.py export-srt --name "DraftName"
```

### Screen recording & smart zoom

```bash
python <SKILL_ROOT>/tools/recording/recorder.py            # Windows
python <SKILL_ROOT>/tools/recording/macos_recorder.py      # macOS

# Apply zoom to existing video
python <SKILL_ROOT>/scripts/jy_wrapper.py apply-zoom --name "Project" --video "v.mp4" --json "e.json"
```

### Template clone & replacer

```bash
# Clone template → new project (never modify the shared template directly)
python <SKILL_ROOT>/scripts/jy_wrapper.py clone --template "酒店模板" --name "客户A_副本"
```

### Movie commentary builder

```bash
python <SKILL_ROOT>/scripts/movie_commentary_builder.py --video "video.mp4" --json "storyboard.json"
```

### Sync native assets

```bash
# Import favorited/played BGM from JianYing App into the skill
python <SKILL_ROOT>/scripts/sync_jy_assets.py
```

### TTS speakers

```bash
python <SKILL_ROOT>/scripts/list_tts_speakers.py
```

### Other scripts

| Script | Purpose |
|---|---|
| `scripts/jy_wrapper.py` | Main `JyProject` wrapper (add_media_safe, add_text_simple, add_styled_text, add_tts_intelligent, add_cloud_media, apply-zoom, clone, export-srt) |
| `scripts/cloud_manager.py` | Cloud asset download/import manager |
| `scripts/web_recorder.py` | Playwright-based web VFX recorder |
| `scripts/smart_zoomer.py` | Smart-zoom keyframe generator from click events |
| `scripts/smart_rough_cut.py` | Rough-cut automation |
| `scripts/universal_tts.py` | Universal TTS backend |
| `scripts/video_analyzer.py` | Video analysis helper |
| `scripts/build_cloud_music_library.py` | Rebuild cloud music CSV index |
| `scripts/build_cloud_text_styles_library.py` | Rebuild flower-text style index |

### Prompts

- `prompts/movie_commentary.md` — Movie commentary storyboard prompt template
- `prompts/readme_to_tutorial.md` — README → tutorial video script (inject into `{{README_CONTENT}}`)

---

## 🚀 Quick-Start Python Template

```python
import os
import sys

# 1. 环境初始化 (必须同步到脚本开头)
current_dir = os.path.dirname(os.path.abspath(__file__))
env_root = os.getenv("JY_SKILL_ROOT", "").strip()
# 探测 Skill 路径 (支持 Antigravity, Trae, Claude 等)
skill_root = next((p for p in [
    env_root,
    os.path.join(current_dir, ".agent", "skills", "jianying-editor"),
    os.path.join(current_dir, ".trae", "skills", "jianying-editor"),
    os.path.join(current_dir, ".claude", "skills", "jianying-editor"),
    os.path.join(current_dir, "skills", "jianying-editor"),
    os.path.abspath(".agent/skills/jianying-editor"),
    os.path.abspath(".trae/skills/jianying-editor"),
    os.path.abspath(".claude/skills/jianying-editor"),
    os.path.dirname(current_dir) # 如果在 examples/ 目录下
] if p and os.path.exists(os.path.join(p, "scripts", "jy_wrapper.py"))), None)

if not skill_root: raise ImportError("Could not find jianying-editor skill root.")
sys.path.insert(0, os.path.join(skill_root, "scripts"))
from jy_wrapper import JyProject

if __name__ == "__main__":
    project = JyProject("My Video Project")
    assets_dir = os.path.join(skill_root, "assets")

    # 2. 导入视频与配乐
    project.add_media_safe(os.path.join(assets_dir, "video.mp4"), "0s")
    project.add_media_safe(os.path.join(assets_dir, "audio.mp3"), "0s", track_name="Audio")

    # 3. 添加带动画的标题
    project.add_text_simple("剪映自动化开启", start_time="1s", duration="3s", anim_in="复古打字机")

    project.save()
```

---

## 📂 Data & Assets

- `data/cloud_music_library.csv` — Cloud BGM index
- `data/cloud_sound_effects.csv` — Cloud SFX index
- `data/cloud_video_assets.csv` — Cloud video clips
- `data/cloud_text_styles.csv` — Flower-text (花字) style IDs
- `data/filters.csv`, `data/transitions.csv`, `data/video_scene_effects.csv`, `data/text_animations.csv`, `data/video_intro_animations.csv`, `data/video_outro_animations.csv` — Asset catalogs
- `data/tts_speakers.csv` — TTS voice catalog
- `data/jy_cached_audio.csv` — Locally synced music paths
- `references/AVAILABLE_ASSETS.md` — Full available-asset catalogue
- `assets/artistEffect/<style_id>/` — Local flower-text effect prefabs

---

## 📖 References

- [references/community-tools.md](references/community-tools.md) — Comparison of all 9 open-source community projects (capcut-mate, pyJianYingDraft, pyCapCut, VectCutAPI, jianying-mcp, capcut-mcp-server, capcut-mcp, capcut-ai-editor, capcut-cli)
- [references/capcut-mate-api.md](references/capcut-mate-api.md) — Full 39-endpoint FastAPI reference for capcut-mate
- [references/draft-json-schema.md](references/draft-json-schema.md) — `draft_content.json` schema: tracks, materials, segments, mirror files, platform markers, JianYing 6.0+ encryption

---

## 🧭 How This Skill Was Built

This integrated skill consolidates desktop automation (`JyProject` over pyJianYingDraft) with draft-JSON knowledge extracted from **9 open-source projects**:

1. **capcut-mate** (Hommy-master) — FastAPI, 39 REST endpoints, MIT
2. **pyJianYingDraft** (GuanYixuan) — Python draft library, Apache 2.0 (the foundation)
3. **pyCapCut** (GuanYixuan) — International CapCut library
4. **VectCutAPI** (sun-guannan) — Cloud API + MCP, 800+ stars, Apache 2.0
5. **jianying-mcp** (hey-jian-wei) — MCP tools, two-phase export, Apache 2.0
6. **capcut-mcp-server** (Atx-Guy) — TypeScript MCP proxy, MIT
7. **capcut-mcp** (fancyboi999) — Python + FFmpeg
8. **capcut-ai-editor / SmartCut** (mrbuslov) — Talking-head smart cut, MIT
9. **capcut-cli** (renezander030) — Zero-dependency Node CLI, 60+ commands, MIT

When a community project exposes a capability JyProject lacks (e.g., capcut-cli's `lint --fix`, SmartCut's gap detection, VectCutAPI's OSS upload), the corresponding rule file documents how to call it. Start at [references/community-tools.md](references/community-tools.md) to choose the right tool for a given need.
