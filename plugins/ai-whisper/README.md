# Whisper Voice Transcription

On-device speech-to-text audio transcription running on Snapdragon 8 Elite NPU / CPU, distributed between your Linux laptop and Android phone over USB.

## Installation

```bash
pocketbridge plugin install ai-whisper
```

> **Note:** This plugin downloads ~75 MB of Whisper model weights. Your phone needs at least 1.5 GB free RAM and 150 MB free storage.

## Usage

```bash
# Transcribe an audio file (splits work between laptop and phone)
pocketbridge run -a ai_whisper --hybrid --input audio.wav

# Run inference on phone only (uses Snapdragon 8 Elite NPU)
pocketbridge run -a ai_whisper --remote-only --input interview.mp3
```

## Model Variants

| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| `whisper-tiny` (default) | 39 MB | Fastest | Good for clear speech |
| `whisper-base` | 74 MB | Fast | Better for noisy environments |

## How It Works

1. Audio file is split into segments on the laptop
2. Phone's Snapdragon 8 Elite NPU processes segments in parallel
3. Transcription results are merged and returned over USB
4. Total latency: ~2-5 seconds for a 30-second audio clip

## Hardware Requirements

- **Host RAM:** 1024 MB minimum
- **Worker RAM:** 1536 MB minimum
- **Worker Storage:** 150 MB minimum (for model weights)

## License

MIT
