<div align="right">

**[English](README.md)** · [简体中文](README.zh-CN.md)

</div>

# jianying-editor-skill

Automating CapCut / JianYing — through the UI, or by writing the draft file directly.

<sub>Skill · `SKILL.md` + prompts + rules</sub>

---

## Two ways in

Editing software can be automated two ways, and they fail differently:

| Approach | Strength | Weakness |
|---|---|---|
| Drive the UI | works on any version | breaks on layout changes, slow, hard to verify |
| Write the draft file | precise, fast, diffable | breaks when the format changes |

This skill keeps both, and documents when to use which. The UI path is for actions the draft format cannot express; the draft path is for anything structural — clip order, timing, transitions.

## What the skill covers

- Draft project structure (the JSON that CapCut reads)
- Clip, track and keyframe representation
- Transition and effect parameters
- Text and subtitle timing
- Export settings

## Layout

```
SKILL.md       the method, 16 KB
docs/          reference notes
prompts/       prompt templates for the agent
rules/         constraints the agent must respect
```

## Using it

Load `SKILL.md` into an agent that supports skill bundles, then point it at a project. `rules/` is not optional reading — it holds the constraints that keep generated edits loadable.

## Notes

Verified against CapCut's draft format as of writing. The format is undocumented and changes; check `docs/` for the versions this was tested on before relying on it.

## License

MIT
