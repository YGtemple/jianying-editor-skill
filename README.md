# jianying-editor-skill · 剪映/CapCut 终极自动化剪辑 Skill（v2.0.0）

> 一句话简介：桌面 UI 自动化 + 草稿 JSON 直接编辑双引擎——花字/关键帧/蒙版/转场/TTS/录屏/Web动效/批量无头导出/智能剪口播全流程规则库。

## 一、项目概述与定位

这是一个带 frontmatter 的 AI Skill（`name: jianying-editor`，v2.0.0），用于在**剪映专业版（JianYing Pro）/ CapCut** 中自动化完成视频剪辑。它采用"**双引擎**"：
1. **桌面自动化**——通过 computer_use 控制剪映桌面应用；
2. **草稿 JSON 直接编辑**——基于 `pyJianYingDraft` 库直接读写 `draft_content.json`，绕过 GUI。

本仓库是该 Skill 的**文档与规则子集**（README 自述"包含完整文档和规则文件"）：提供主入口路由、规则模块、操作手册与提示词模板；README/SKILL 中提到的 `scripts/`、`examples/`、`data/`、`references/`、`tools/` 等可执行脚本与资源目录**未包含在本仓库**（属完整 Skill 的其余部分）。

它整合了从 **9 个开源社区项目**提炼的草稿结构知识：capcut-mate、pyJianYingDraft、pyCapCut、VectCutAPI、jianying-mcp、capcut-mcp-server、capcut-mcp、capcut-ai-editor/SmartCut、capcut-cli。

## 二、核心能力

自动化剪辑全流程：素材导入、字幕/花字/SRT、关键帧动画（缩放/平移/透明度/旋转，Ken Burns）、滤镜/转场/场景特效/人物特效/蒙版、TTS 配音 + BGM/SFX 配乐与混音、屏幕录制（Win/macOS）+ 智能推镜关键帧、Web 动效合成（HTML/JS/Canvas → 透明 MP4，经 Playwright）、生成式/AI 剪辑与影视解说、草稿 JSON 深度检视、**批量/无头导出**（自动部署、热重载、OSS 上传、ffmpeg 代理渲染）、**智能剪口播**（按字幕间隙去静音、重复镜头检测、原地编辑草稿）。

## 三、技术栈与开发铁律

- Python 3.10+、`pyJianYingDraft`（草稿 JSON 操作库，Apache 2.0）、剪映专业版/CapCut、Windows 为主/macOS；依赖 `python3`、`ffmpeg`，环境变量 `JY_SKILL_ROOT`。
- **关键开发原则**：
  1. 剪辑脚本**不写在 Skill 内部目录**，须放用户项目根目录，保持 Skill 纯净可移植；
  2. 简单演示用默认配乐，实际项目优先查 `data/cloud_music_library.csv` 按主题推荐；
  3. `JyProject` 初始化按主素材比例设分辨率（默认横屏 1920×1080，竖屏显式 1080×1920）；
  4. 脚本末尾**必须 `project.save()`**；
  5. **不猜资源 ID**（滤镜/转场/特效随版本变），必须先用 `asset_search.py` 搜索。

## 四、目录结构（本仓库实际内容）

```
jianying-editor-skill/
├── SKILL.md                 # 主入口路由器（16KB：路由表/执行手册/铁律/脚本清单/Quick-Start 模板）
├── README.md                # 项目简介与结构说明
├── docs/                    # 操作文档
│   ├── agent-playbook.md    # Agent 执行手册（Quick Edit 模板 + 验收清单）
│   ├── minimal-command-sop.md  # 最小命令 SOP
│   └── api.md               # API 参考
├── prompts/                 # 提示词模板
│   ├── movie_commentary.md  # 影视解说分镜生成
│   └── readme_to_tutorial.md  # README → 教程视频脚本（{{README_CONTENT}} 占位）
└── rules/                   # 规则模块（11 个）
    ├── setup.md             # 每个脚本必做的 Python bootstrap / sys.path 解析
    ├── core.md              # JyProject 核心操作（建项目/save/模板克隆/自动导出/验收清单，16KB）
    ├── cli.md               # CLI 契约（draft inspector、诊断、asset search、--json 输出）
    ├── media.md             # 媒体导入、云素材、AI 视频分析优化、比例规则
    ├── text.md              # 纯文本/花字/SRT 导入/自动分层/字幕定位
    ├── keyframes.md         # 关键帧动画（KeyframeProperty）
    ├── effects.md           # 滤镜/转场/场景特效/人物特效搜索与应用
    ├── audio-voice.md       # TTS 配音、旁白字幕、BGM/SFX 来源与混音
    ├── recording.md         # 录屏（Win/macOS）+ 智能推镜关键帧自动生成
    ├── web-vfx.md           # Web→视频 VFX（Playwright 录透明 MP4）
    └── generative.md        # 生成式/AI 剪辑思维链、影视解说构建器
```

> 说明：README 还列出 `draft-structure.md`、`batch-export.md`、`talking-head.md` 三个规则与 `references/`（community-tools/capcut-mate-api/draft-json-schema），但这些**未出现在本仓库实际目录**，属完整 Skill 的其余部分。

## 五、关键内容解读

- **`SKILL.md`（路由器）**：是整个 Skill 的单入口。给出"Agent Quick Routing Table"——按场景（剪辑/字幕花字/关键帧/特效/TTS/录屏/Web VFX/生成式/草稿检视/批量导出/智能剪口播/诊断）路由到对应 rule 文件；列出命名工作流（云视频+云音乐+TTS、旁白+字幕对齐、录屏+智能推镜、批量导出、影视解说、复合嵌套片段、AI 转写→匹配 B-roll→组装）；附 Quick-Start Python 模板（自动探测 `.agent/.trae/.claude/skills/jianying-editor` 路径并 `from jy_wrapper import JyProject`）。
- **`rules/core.md`**：最大规则文件，封装 `JyProject` 的建项目、保存、模板克隆、自动导出与验收清单。
- **`rules/setup.md`**：规定每个脚本开头必须做的环境探测与 `sys.path` 注入（与 SKILL.md 的 Quick-Start 模板一致）。
- **`prompts/`**：两个提示词模板——影视解说分镜、文档转教程脚本。
- **知识来源**：SKILL.md 末尾详述 9 个社区项目各自特点与许可证，说明"当社区项目有 JyProject 缺的能力（如 capcut-cli 的 lint --fix、SmartCut 的间隙检测、VectCutAPI 的 OSS 上传）时，对应规则文件教你如何调用"。

## 六、运行与使用

本仓库本身是规则/文档，无可运行脚本。完整使用时把 Skill 放入 Agent skills 目录，Agent 按 SKILL.md 路由；剪辑脚本按 Quick-Start 模板探测 `JY_SKILL_ROOT` 后调用 `JyProject`。典型命令（完整 Skill 中）：`asset_search.py "复古" -c filters`、`draft_inspector.py show --name X --kind content --json`、`auto_exporter.py "Draft" out.mp4 --res 1080 --fps 60`。

## 七、数据/资源构成

本仓库全部为**文本文件**（Markdown），无图片、音视频、字体、压缩包等二进制文件；提到的 `data/*.csv` 资源目录与 `assets/` 预制件不在本仓库。

## 八、项目特点

1. **规则驱动的自动化**：把"怎么用代码剪辑剪映"沉淀成可路由的规则库，而非裸脚本集合。
2. **双引擎互补**：GUI 自动化与草稿 JSON 直编结合，兼顾可观察性与可批量性。
3. **整合社区智慧**：跨 9 个开源项目取长补短并标注许可证，避免重复造轮子。
4. **面向 Agent 工程**：强调脚本隔离、资源 ID 必查、必 save、分辨率显式等防错纪律。
