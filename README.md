# jianying-editor-skill

剪映 (JianYing / CapCut) 终极自动化剪辑 Skill — 桌面自动化 + 草稿 JSON 直接编辑双引擎。

## 仓库内容

本仓库包含 jianying-editor skill v2.0.0 的完整文档和规则文件：

```
├── SKILL.md                          # 技能主入口和路由
├── README.md                         # 本文件
├── rules/                            # 12 个规则模块
│   ├── setup.md                     # 环境配置和依赖
│   ├── cli.md                        # CLI 工具和命令行接口
│   ├── core.md                       # 核心 API (JyProject 封装)
│   ├── media.md                     # 媒体导入和管理
│   ├── text.md                      # 文字/字幕/花字
│   ├── keyframes.md                  # 关键帧动画
│   ├── effects.md                   # 特效/滤镜/转场/蒙版
│   ├── audio-voice.md               # 音频/TTS/配音
│   ├── recording.md                  # 录屏和录制
│   ├── web-vfx.md                   # Web 特效素材
│   ├── generative.md               # 生成式剪辑
│   ├── draft-structure.md          # 草稿 JSON 结构完整参考
│   ├── batch-export.md            # 批量导出和无头自动化
│   └── talking-head.md            # 智能剪口播/去静音
├── references/                       # 参考文档
│   ├── README.md
│   ├── community-tools.md           # 社区工具生态
│   ├── capcut-mate-api.md          # CapCut Mate API 参考
│   └── draft-json-schema.md        # 草稿 JSON Schema
├── docs/                             # 操作文档
│   ├── agent-playbook.md           # Agent 执行手册
│   ├── minimal-command-sop.md      # 最小命令 SOP
│   └── api.md                       # API 参考
└── prompts/                          # 提示词模板
    ├── movie_commentary.md         # 影视解说生成
    └── readme_to_tutorial.md        # 文档转教程
```

## 安装

将此 skill 复制到你的 skills 目录：

```bash
# 克隆仓库
git clone https://github.com/YGtemple/jianying-editor-skill.git

# 复制到 skills 目录
cp -r jianying-editor-skill /path/to/your/skills/jianying-editor
```

## 核心能力

- **桌面自动化**: 通过 computer_use 控制剪映桌面应用
- **草稿 JSON 编辑**: 直接读写 draft_content.json，绕过 GUI
- **批量导出**: 无头模式批量处理
- **智能剪口播**: 自动去除静音和重复片段
- **TTS 配音**: 内置语音合成
- **特效/转场**: 完整的资源 ID 数据库

## 技术栈

- Python 3.10+
- pyJianYingDraft (草稿 JSON 操作库)
- 剪映专业版 (JianYing Pro) / CapCut
- Windows (主要支持) / macOS

## 致谢

本技能整合了以下开源项目的知识：
- [pyJianYingDraft](https://github.com/ChineseYukina/pyJianYingDraft)
- [capcut-mate](https://github.com/...) 
- [capcut-cli](https://github.com/renezander030)
- [pyCapCut](https://github.com/Hadas7/pyCapCut)

## License

MIT
