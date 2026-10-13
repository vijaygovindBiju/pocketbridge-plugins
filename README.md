# PocketBridge Plugin & AI Model Hub

Official community plugin registry for [PocketBridge](https://github.com/vijaygovindBiju/PocketBridge) — the USB-to-USB compute offloading engine between your Linux laptop and Android phone.

## What Is This?

This repository hosts **downloadable plugin packages** that extend PocketBridge with new workloads — image processing, data analytics, AI speech transcription, headless browser rendering, video transcoding, and more.

The core PocketBridge engine ships with only one built-in adapter (`chunk_transform.v1`) to keep the binary small (~50 KB). Everything else is installed on-demand from this hub.

## Quick Start

```bash
# List all available plugins
pocketbridge plugin list

# Install a plugin (downloads to ~/.pocketbridge/plugins/ and syncs to phone over USB)
pocketbridge plugin install image-batch

# Use it immediately
pocketbridge run -a image_batch --local-only --records 100

# View plugin details and hardware requirements
pocketbridge plugin info ai-whisper

# Remove a plugin
pocketbridge plugin remove image-batch
```

## Available Plugins

| Plugin | Version | Size | Description |
|--------|---------|------|-------------|
| **image-batch** | v1.0.0 | 1.2 MB | High-throughput parallel image resizing, inversion, and box-blur filtering |
| **data-analytics** | v1.0.0 | 0.8 MB | Structured CSV/TSV table reduction, sum, min/max, and numerical filtering |
| **ai-whisper** | v1.1.0 | 71.5 MB | On-device speech-to-text transcription on Snapdragon 8 Elite NPU |
| **browser-cdp** | v0.9.0 | 14.3 MB | Offloads inactive browser tabs to Android WebView engine |
| **media-ffmpeg** | v1.2.0 | 33.4 MB | Distributed H.264/AV1 video transcoding across Intel QuickSync and Snapdragon GPU |

## How It Works

```
┌──────────────────────────────────────────────────────────────────┐
│  pocketbridge plugin install ai-whisper                         │
│                                                                  │
│  [1/4] Inspecting host resources... (15688 MB RAM)    [OK]      │
│  [2/4] Querying worker phone over ADB...              [OK]      │
│  [3/4] Pre-flight hardware requirement gates...       [OK]      │
│  [4/4] Installing plugin to host...                   [OK]      │
│  [Sync] Transferred to Galaxy S25 Ultra over USB      [OK]      │
│                                                                  │
│  Plugin 'Whisper Voice Transcription' (v1.1.0) installed!       │
│  Adapter 'ai_whisper.v1' is now active.                         │
└──────────────────────────────────────────────────────────────────┘
```

**Pre-flight safety gates** automatically check:
- Host laptop RAM meets minimum requirement
- Android phone RAM is sufficient for the plugin
- Phone storage has enough free space for model weights
- SHA-256 cryptographic hash verification of all downloaded payloads

## Plugin Package Structure

Each plugin is a directory containing:

```
plugins/image-batch/
├── manifest.json          # Metadata, requirements, checksums
├── README.md              # Usage documentation
└── releases/
    └── v1.0.0/
        ├── linux-x86_64/
        │   └── libimage_batch.so        # Host Linux shared library
        └── android-arm64/
            └── libimage_batch_arm.so    # Worker Android NDK binary
```

## Creating Your Own Plugin

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide. In brief:

1. Fork this repository
2. Create your plugin directory under `plugins/your-plugin-name/`
3. Write a `manifest.json` following the schema below
4. Build pre-compiled binaries for Linux x86_64 and Android ARM64
5. Compute SHA-256 checksums
6. Submit a Pull Request

Your plugin will be reviewed and, once approved, becomes available to all PocketBridge users worldwide.

**Important:** Your plugin PR only changes this repository. The core PocketBridge engine source code is never modified.

## Hardware Targets

| Platform | Architecture | Tested Device |
|----------|-------------|---------------|
| **Host** | x86_64 (Intel/AMD) | 13th Gen Intel Core i5-13420H, Arch Linux |
| **Worker** | aarch64 (ARM64) | Samsung Galaxy S25 Ultra, Snapdragon 8 Elite |
| **Link** | USB-C (physical cable, no Wi-Fi) | ADB forward tcp:9876 |

## License

Individual plugins may have their own licenses (specified in each `manifest.json`). The registry infrastructure is licensed under Apache-2.0.
