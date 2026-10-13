# FFmpeg Parallel Media Splitter

Distributed H.264/AV1 video transcoding that splits encoding work across your Intel laptop (QuickSync) and Snapdragon phone (GPU) over USB.

## Installation

```bash
pocketbridge plugin install media-ffmpeg
```

> **Note:** This plugin is ~35 MB. Your phone needs at least 1 GB free RAM and 100 MB free storage.

## How It Works

1. Input video is split into temporal segments on the laptop
2. Segments are distributed to both laptop (Intel QuickSync) and phone (Snapdragon GPU)
3. Encoded segments are merged back on the laptop
4. Result: **up to 1.8x faster** encoding than laptop alone

## Usage

```bash
# Transcode a video using both devices
pocketbridge run -a media_ffmpeg --hybrid --input video.mp4 --output output.mp4

# Encode on phone only (useful when laptop is under heavy load)
pocketbridge run -a media_ffmpeg --remote-only --input raw_footage.mov

# Specify codec
pocketbridge run -a media_ffmpeg --hybrid --codec av1 --input 4k_clip.mp4
```

## Supported Codecs

| Codec | Host Acceleration | Phone Acceleration |
|-------|-------------------|-------------------|
| H.264 | Intel QuickSync | Snapdragon Adreno GPU |
| H.265/HEVC | Intel QuickSync | Snapdragon Adreno GPU |
| AV1 | Software (SVT-AV1) | Snapdragon NPU (experimental) |

## Hardware Requirements

- **Host RAM:** 1024 MB minimum
- **Worker RAM:** 1024 MB minimum
- **Worker Storage:** 100 MB minimum (for codec libraries)

## License

GPL-3.0
