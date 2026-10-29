# PocketBridge Plugin & AI Model Hub ⚡

> **Official community plugin & compute workload registry for [PocketBridge](https://github.com/vijaygovindBiju/PocketBridge)**  
> *Transform idle Android hardware into active, hardware-accelerated coprocessors over high-speed USB.*

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](LICENSE)
[![Platform: Linux x86_64](https://img.shields.io/badge/Host-Linux%20x86__64-orange.svg)]()
[![Platform: Android ARM64](https://img.shields.io/badge/Worker-Android%20ARM64-brightgreen.svg)]()
[![Transport: USB](https://img.shields.io/badge/Link-USB--C%20(AOA%20%2F%20ADB)-informational.svg)]()

---

## 📌 What Is This?

This repository hosts **verified, downloadable plugin packages** that extend [PocketBridge](https://github.com/vijaygovindBiju/PocketBridge) with high-performance offloaded workloads—including transformer LLM inference, on-device Whisper voice transcription, distributed video transcoding, parallel image processing, and headless browser rendering.

The core PocketBridge daemon ships with a lightweight built-in adapter to maintain a minimal binary footprint (~50 KB). All specialized compute workloads and neural models are installed on-demand from this hub.

---

## 🚀 Quick Start

```bash
# List all available community plugins
pocketbridge plugin list

# Install a plugin (verifies SHA-256 and syncs to phone over USB)
pocketbridge plugin install llm-llama

# Execute an offloaded workload immediately
pocketbridge run -a llm_llama --prompt "Explain quantum computing in 3 sentences."

# Inspect plugin hardware requirements and model weights
pocketbridge plugin info ai-whisper

# Uninstall a plugin
pocketbridge plugin remove image-batch
```

---

## 📦 Available Plugins

| Plugin | Version | Binary / Model Size | Description | Hardware Target |
| :--- | :--- | :--- | :--- | :--- |
| **`llm-llama`** | `v1.0.0` | 185.0 MB | On-device quantized GGUF transformer text generation & completion | Qualcomm Hexagon NPU / Adreno GPU |
| **`ai-whisper`** | `v1.1.0` | 71.5 MB | On-device speech-to-text audio transcription | Qualcomm Hexagon NPU / CPU |
| **`media-ffmpeg`** | `v1.2.0` | 33.4 MB | Distributed H.264/AV1 hardware-accelerated video transcoding | Intel QuickSync + Snapdragon GPU |
| **`browser-cdp`** | `v0.9.0` | 14.3 MB | Offloads inactive browser tabs and DOM trees to Android WebView | Android WebKit Engine |
| **`image-batch`** | `v1.0.0` | 1.2 MB | Parallel batch image resizing, color inversion, and box-blur filtering | Multi-Core ARM64 NEON |
| **`data-analytics`**| `v1.0.0` | 0.8 MB | Structured CSV/TSV table reduction, sum, min/max, and numerical filtering | Vectorized SIMD |
| **`phone-cluster`** | `v1.0.0` | 1.2 MB | Multi-device USB farm orchestration with load balancing and failover | Multi-USB Controller Hub |

---

## ⚙️ How Installation & Verification Works

```text
┌──────────────────────────────────────────────────────────────────┐
│  pocketbridge plugin install llm-llama                          │
│                                                                  │
│  [1/4] Inspecting host resources... (16 GB RAM)       [OK]      │
│  [2/4] Querying worker phone over USB...              [OK]      │
│  [3/4] Pre-flight hardware requirement gates...       [OK]      │
│  [4/4] Installing host adapter library...             [OK]      │
│  [Sync] Transferred to Galaxy S25 Ultra over USB      [OK]      │
│                                                                  │
│  Plugin 'Llama LLM Inference' (v1.0.0) installed!               │
│  Adapter 'llm_llama.v1' is now active.                          │
└──────────────────────────────────────────────────────────────────┘
```

**Automated Pre-Flight Safety Gates:**
- **Host & Phone Memory Checks:** Verifies minimum RAM thresholds before initiating download.
- **Storage Availability:** Confirms sufficient free space on device flash memory.
- **Cryptographic Hash Verification:** Validates SHA-256 checksums of all native binaries and quantized model weights before execution.
- **Architecture Validation:** Guarantees ABI compatibility (`x86_64` for host, `arm64-v8a` for Android coprocessor).

---

## 📂 Plugin Package Structure

Each plugin directory follows a standardized schema:

```text
plugins/image-batch/
├── manifest.json          # Metadata, hardware requirements, entrypoints, SHA-256 checksums
├── README.md              # Documentation, benchmark metrics, CLI arguments
└── releases/
    └── v1.0.0/
        ├── linux-x86_64/
        │   └── libimage_batch.so        # Host Linux shared library
        └── android-arm64/
            └── libimage_batch_arm.so    # Worker Android NDK shared library
```

---

## 🛠️ Contributing a Plugin

We welcome community-contributed plugins! Please refer to **[CONTRIBUTING.md](CONTRIBUTING.md)** for our full development guide.

1. Fork this repository.
2. Create your plugin directory under `plugins/<your-plugin-name>/`.
3. Provide a valid `manifest.json` referencing your compiled binaries.
4. Compile pre-built binaries for `linux-x86_64` and `android-arm64`.
5. Compute and attach SHA-256 cryptographic hashes.
6. Submit a Pull Request.

---

## 📄 License

Individual plugins declare their own licenses within their respective `manifest.json`. The hub infrastructure and plugin registry manifest are licensed under the **[Apache License 2.0](LICENSE)**.
