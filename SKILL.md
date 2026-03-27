---
name: insanely-fast-whisper
description: >
  Blazingly fast local speech-to-text using insanely-fast-whisper. Transcribes
  audio files up to 10-30x faster than standard Whisper by leveraging GPU
  acceleration (Apple Silicon MPS on Mac, CUDA on Nvidia). No API key needed.
  Use when: (1) transcribing audio or video files locally, (2) transcribing
  long recordings quickly, (3) a faster alternative to openai-whisper,
  (4) voice messages, podcasts, meetings, or any speech-to-text task.
when: "User wants to transcribe audio, convert speech to text, transcribe a file, voice message, podcast, meeting recording, or needs fast local Whisper transcription"
examples:
  - "Transcribe this audio file"
  - "Convert this voice message to text"
  - "Transcribe my meeting recording"
  - "Transcribe audio.mp3 to a text file"
  - "Transcribe this podcast episode"
  - "Use insanely-fast-whisper to transcribe interview.m4a"
homepage: https://github.com/jamesalmeida/insanely-fast-whisper-skill
metadata: {"openclaw": {"emoji": "⚡", "requires": {"bins": ["insanely-fast-whisper"]}, "primaryEnv": null}}
---

# insanely-fast-whisper

Local speech-to-text powered by [insanely-fast-whisper](https://github.com/Vaibhavs10/insanely-fast-whisper) — up to 10-30x faster than standard Whisper via HuggingFace Transformers and GPU acceleration.

## Installation

Install the CLI with pipx (recommended):

```bash
pipx install insanely-fast-whisper
```

On macOS with Python 3.11:

```bash
pipx install insanely-fast-whisper --force --pip-args="--ignore-requires-python"
```

Verify install:

```bash
insanely-fast-whisper --help
```

## Quick Start

**Transcribe a file (macOS — uses Apple Silicon GPU via MPS):**

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps
```

**Transcribe on Linux/Nvidia GPU:**

```bash
insanely-fast-whisper --file-name audio.mp3
```

**Save output to a file:**

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps --transcript-path output.json
```

**Use whisper-large-v3 with Flash Attention 2 (fastest, requires compatible GPU):**

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps --flash True
```

**Use distil-whisper (smaller, faster, slightly less accurate):**

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps --model-name distil-whisper/large-v2
```

**Batch processing (faster for multiple/long files):**

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps --batch-size 24
```

## Key Options

| Flag | Default | Description |
|------|---------|-------------|
| `--file-name` | required | Path to audio file or URL |
| `--device-id` | `0` (CUDA) | `mps` for Apple Silicon, `0` for Nvidia GPU, `cpu` for CPU-only |
| `--model-name` | `openai/whisper-large-v3` | HuggingFace model name |
| `--batch-size` | `24` | Batch size for processing (higher = faster on GPU) |
| `--flash` | `False` | Enable Flash Attention 2 (requires flash-attn package) |
| `--transcript-path` | `output.json` | Where to save the transcript JSON |
| `--language` | auto-detect | Force language (e.g. `en`, `es`, `fr`) |
| `--task` | `transcribe` | `transcribe` or `translate` (to English) |

## Supported Formats

Works with any audio/video format that ffmpeg can handle:
- `.mp3`, `.mp4`, `.m4a`, `.wav`, `.ogg`, `.webm`, `.flac`, `.mov`

## Models

| Model | Size | Speed | Notes |
|-------|------|-------|-------|
| `openai/whisper-large-v3` | ~3GB | Fast | Best accuracy (default) |
| `distil-whisper/large-v2` | ~1.5GB | Faster | Great accuracy, half the size |
| `distil-whisper/medium.en` | ~800MB | Fastest | English-only, very lightweight |
| `openai/whisper-medium` | ~1.5GB | Medium | Good balance |

Models download to `~/.cache/huggingface/hub/` on first use.

## Output Format

Output is JSON (`output.json` by default):

```json
{
  "speakers": [],
  "chunks": [
    {"timestamp": [0.0, 3.5], "text": "Hello world"},
    {"timestamp": [3.5, 7.2], "text": "This is a transcription"}
  ],
  "text": "Hello world This is a transcription"
}
```

The `text` field contains the full transcript.

## Workflows

**Transcribe and extract plain text:**

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps --transcript-path /tmp/out.json
cat /tmp/out.json | python3 -c "import json,sys; print(json.load(sys.stdin)['text'])"
```

**Transcribe with speaker diarization** (requires HuggingFace token):

```bash
insanely-fast-whisper --file-name audio.mp3 --device-id mps --hf-token YOUR_HF_TOKEN --diarization_model pyannote/speaker-diarization-3.1
```

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `command not found: insanely-fast-whisper` | Run `pipx install insanely-fast-whisper` |
| Slow on Mac | Make sure `--device-id mps` is set |
| CUDA errors | Try `--device-id cpu` or check GPU drivers |
| Old version (0.0.8) with Python 3.11 | Use `--pip-args="--ignore-requires-python"` |
| Out of memory | Reduce `--batch-size` (try `8` or `4`) |
| Model not found | Check model name on huggingface.co |
