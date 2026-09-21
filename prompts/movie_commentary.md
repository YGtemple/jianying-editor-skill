# Movie Commentary Builder Prompt

Use this prompt when asking the AI to generate movie commentary (影视解说) content from a long video.

## Input Requirements

- Source video: <= 30 minutes, 360p preferred
- Output: storyboard JSON with scenes, narration text, and timing

## Processing Pipeline

1. Pre-optimize video (compress to 360p, extract audio)
2. Transcribe audio with ASR
3. Analyze scenes and extract key moments
4. Generate narration script
5. Build storyboard JSON
6. Apply to JianYing draft

## Output Format

```json
{
  "title": "解说标题",
  "scenes": [
    {
      "start": 0,
      "end": 10,
      "narration": "解说文字",
      "emotion": "紧张/温馨/悬疑"
    }
  ]
}
```
