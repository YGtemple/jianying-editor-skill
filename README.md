# JianYing Editor Skill v2.0.0

**剪映 / CapCut 终极自动化剪辑 Skill** — 桌面 UI 自动化 + 草稿 JSON 结构操作 + 批量无头导出 + 说话人智能剪辑 + 字幕/特效/转场/TTS/BGM 全流程。

通过自然语言告诉 AI 你想做什么视频，它帮你完成从写文案、配音、加字幕、选音乐、上特效到最终导出的整套流程。

---

## 功能特性

### 核心能力

- **素材导入** — 视频、音频、图片一句话丢进时间轴，支持云端素材
- **AI 配音 (TTS)** — 输入文案自动生成语音，智能音色选择
- **字幕生成** — 根据配音自动拆句、逐句对齐，支持花字/入场动画
- **自动配乐** — 本地音乐或剪映云端曲库（12个CSV索引）
- **特效/转场/滤镜** — 按名字搜索剪映自带特效库，35+转场/30+滤镜
- **关键帧动画** — 缩放、平移、透明度、旋转，Ken Burns 效果
- **录屏 + 智能变焦** — 录制屏幕并自动添加缩放和红圈标记
- **网页动效转视频** — HTML/JS/Canvas 动画录屏变透明视频素材
- **自动导出** — 一键导出 MP4 (1080P~4K)，支持无头批量导出

### v2.0.0 新增能力（整合9个开源项目）

- **草稿 JSON 深度操作** — `draft_content.json` 字段级 schema，轨道/片段/关键帧/蒙版完整结构
- **批量无头导出** — 自动部署、桌面热重载、OSS 上传、ffmpeg 代理渲染
- **说话人智能剪辑 (Talking Head)** — 静音间隙检测、重复片段去除、时间线手术
- **39个 capcut-mate API 完整清单** — 草稿管理/素材/字幕/转场/关键帧/蒙版/云端渲染
- **花字系统** — 60+入场动画、TextStyle/Shadow/Border 参数完整说明
- **色度键/混合模式** — 绿幕抠像、10种混合模式、6种蒙版类型

---

## 项目结构

```
jianying-editor/
├── SKILL.md                    # 主路由入口（AI 说明书）
├── README.md                    # 本文件
├── rules/                      # 14个规则文件
│   ├── setup.md                # Python 环境初始化
│   ├── core.md                 # 核心操作：项目/保存/导出/草稿结构
│   ├── cli.md                  # CLI 命令规范
│   ├── media.md                # 素材导入
│   ├── text.md                 # 文本/花字/字幕
│   ├── keyframes.md             # 关键帧动画
│   ├── effects.md               # 特效/滤镜/转场/蒙版
│   ├── recording.md            # 屏幕录制
│   ├── web-vfx.md              # 网页动效合成
│   ├── generative.md           # AI 生成式剪辑
│   ├── audio-voice.md          # TTS/BGM/音效
│   ├── draft-structure.md      # 草稿 JSON 完整结构（新增）
│   ├── batch-export.md         # 批量无头导出（新增）
│   └── talking-head.md         # 说话人智能剪辑（新增）
├── references/                 # 深度参考文档
│   ├── draft-json-schema.md   # 草稿 JSON 字段级 schema
│   ├── capcut-mate-api.md      # 39个 API 完整清单
│   ├── community-tools.md      # 9个开源项目对比
│   └── AVAILABLE_ASSETS.md     # 可用资源目录（2960行）
├── docs/                       # 操作手册
│   ├── agent-playbook.md       # Agent 执行手册
│   ├── minimal-command-sop.md # 最小命令 SOP
│   └── api.md                  # API 参考
├── examples/                    # 11个 Python 示例脚本
├── scripts/                    # 20+ 自动化脚本
│   ├── jy_wrapper.py           # 主封装库
│   ├── auto_exporter.py        # 自动导出
│   ├── asset_search.py         # 资源搜索
│   ├── draft_inspector.py      # 草稿检查
│   └── vendor/pyJianYingDraft/ # 草稿操作底层库
├── data/                       # 12个资源目录 CSV
│   ├── cloud_music_library.csv
│   ├── cloud_sound_effects.csv
│   ├── filters.csv
│   ├── transitions.csv
│   └── ...
├── prompts/                    # 提示词模板
└── tools/                      # 工具脚本
```

---

## 整合来源

本 skill 整合了以下 9 个开源社区项目的知识：

| 项目 | 作者 | 贡献 |
|------|------|------|
| capcut-mate | Hommy-master | 39个API、FastAPI、蒙版/花字 |
| pyJianYingDraft | GuanYixuan | 草稿操作底层库（基础） |
| pyCapCut | GuanYixuan | 国际版 CapCut 库 |
| VectCutAPI | sun-guannan | 云端API、MCP双协议 |
| jianying-mcp | hey-jian-wei | MCP工具、两阶段导出 |
| capcut-mcp-server | Atx-Guy | TypeScript MCP 代理 |
| capcut-mcp | fancyboi999 | Python + FFmpeg |
| capcut-ai-editor | mrbuslov | Talking head 智能剪辑 |
| capcut-cli | renezander030 | 零依赖 Node CLI |

---

## 支持环境

| 平台 | 状态 | 说明 |
|:----:|:----:|------|
| Windows | ✅ 完全支持 | 包括自动导出（uiautomation） |
| macOS | ✅ 支持 | 导出需通过 Windows Agent |

### 依赖要求

- Python 3.8+
- ffmpeg（录屏功能必需）
- 剪映专业版（自动导出需要 5.9 或更低版本）

---

## 快速开始

### 安装

```bash
cd <workspace>/skills
git clone https://github.com/YGtemple/jianying-editor-skill.git jianying-editor
cd jianying-editor
pip install -r requirements.txt
```

### 设置环境变量

```bash
export JY_SKILL_ROOT=/path/to/jianying-editor
```

### 第一个剪辑脚本

```python
import os, sys

current_dir = os.path.dirname(os.path.abspath(__file__))
env_root = os.getenv("JY_SKILL_ROOT", "").strip()
skill_root = next((p for p in [
    env_root,
    os.path.join(current_dir, ".agent/skills/jianying-editor"),
    os.path.join(current_dir, "skills/jianying-editor"),
] if p and os.path.exists(os.path.join(p, "scripts", "jy_wrapper.py"))), None)

if not skill_root:
    raise ImportError("Could not find jianying-editor skill root.")

sys.path.insert(0, os.path.join(skill_root, "scripts"))
from jy_wrapper import JyProject

project = JyProject("My First Video")
assets_dir = os.path.join(skill_root, "assets")

project.add_media_safe(os.path.join(assets_dir, "video.mp4"), "0s")
project.add_text_simple("Hello World", start_time="1s", duration="3s")
project.save()
```

---

## Agent 使用方式

把以下内容发给 AI Agent（如 Claude / Cursor / Trae 等）：

> 帮我安装 jianying-editor skill：
> ```
> cd <workspace>/skills && git clone https://github.com/YGtemple/jianying-editor-skill.git jianying-editor && cd jianying-editor && pip install -r requirements.txt
> ```
> 然后帮我剪辑一个视频，导入素材，加字幕和BGM。

AI Agent 会自动读取 `SKILL.md`，根据你的需求路由到对应的规则文件。

---

## 许可证

MIT License

## 相关链接

- [GitHub 仓库](https://github.com/YGtemple/jianying-editor-skill)
- [原始项目 isYangs/jianying-editor-skill](https://github.com/isYangs/jianying-editor-skill)
