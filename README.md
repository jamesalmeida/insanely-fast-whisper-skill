# insanely-fast-whisper OpenClaw Skill

An [OpenClaw](https://openclaw.ai) agent skill for blazingly fast local speech-to-text using [insanely-fast-whisper](https://github.com/Vaibhavs10/insanely-fast-whisper).

> Transcribe 2.5 hours of audio in under 2 minutes — locally, no API key needed.

## Features

- ⚡ **10-30x faster** than standard Whisper via GPU acceleration
- 🍎 **Apple Silicon support** — uses Metal (MPS) backend on Mac
- 🔒 **100% local** — no data sent to external APIs
- 🤗 **HuggingFace models** — whisper-large-v3, distil-whisper, and more
- 📝 **Multiple formats** — mp3, mp4, m4a, wav, ogg, webm, flac, mov

## Quick Start

1. **Install the CLI:**
   ```bash
   pipx install insanely-fast-whisper
   ```

2. **Install this skill:**
   ```bash
   openclaw skills install insanely-fast-whisper
   ```
   Or clone into your workspace skills folder.

3. **Ask your agent:**
   > "Transcribe this audio file: ~/Downloads/recording.m4a"

## Example Commands

```bash
# macOS (Apple Silicon GPU)
insanely-fast-whisper --file-name audio.mp3 --device-id mps

# Save transcript to file
insanely-fast-whisper --file-name audio.mp3 --device-id mps --transcript-path output.json

# Faster distil-whisper model
insanely-fast-whisper --file-name audio.mp3 --device-id mps --model-name distil-whisper/large-v2

# Translate to English
insanely-fast-whisper --file-name audio.mp3 --device-id mps --task translate
```

## Requirements

- Python 3.8+ with pipx
- `insanely-fast-whisper` CLI installed
- ffmpeg (usually pre-installed on macOS, or `brew install ffmpeg`)
- Apple Silicon Mac (for MPS acceleration) or Nvidia GPU (for CUDA)

## vs. openai-whisper

| | openai-whisper | insanely-fast-whisper |
|---|---|---|
| Speed | Baseline | 10-30x faster (GPU) |
| Install | `brew install openai-whisper` | `pipx install insanely-fast-whisper` |
| GPU | Limited | Full MPS/CUDA support |
| Models | whisper-* | whisper-* + distil-whisper |
| API key | Not needed | Not needed |
| Output | txt/srt/vtt/json | JSON with timestamps |

## Credits

- [insanely-fast-whisper](https://github.com/Vaibhavs10/insanely-fast-whisper) by Vaibhav Srivastav
- [OpenAI Whisper](https://openai.com/research/whisper) models
- Powered by [HuggingFace Transformers](https://huggingface.co/docs/transformers)

## License

MIT
