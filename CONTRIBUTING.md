# Contributing to PocketBridge Plugins

Thank you for your interest in creating a plugin for PocketBridge! This guide walks you through the entire process.

## Before You Start

Make sure you have:
- [PocketBridge](https://github.com/vijaygovindBiju/PocketBridge) installed and working on your machine
- Rust toolchain (`rustup`, `cargo`) for native plugins
- Android NDK (for cross-compiling to ARM64, if your plugin runs on the phone)
- A Samsung Galaxy S25 Ultra or other ARM64 Android device for testing (recommended)

## Step-by-Step Guide

### 1. Fork & Clone This Repository

```bash
# Fork on GitHub, then:
git clone https://github.com/YOUR_USERNAME/pocketbridge-plugins.git
cd pocketbridge-plugins
```

### 2. Create Your Plugin Directory

```bash
mkdir -p plugins/my-awesome-plugin/releases/v1.0.0
```

### 3. Write Your `manifest.json`

Create `plugins/my-awesome-plugin/manifest.json`:

```json
{
  "id": "my-awesome-plugin",
  "name": "My Awesome Plugin",
  "version": "1.0.0",
  "description": "A brief description of what your plugin does",
  "author": "Your Name",
  "license": "MIT",
  "kind": "Native",
  "adapter_id": "my_awesome.v1",
  "requirements": {
    "min_host_ram_mb": 256,
    "min_worker_ram_mb": 128,
    "min_worker_storage_mb": 20
  },
  "sha256": null,
  "size_bytes": 0,
  "host_entrypoint": "my_awesome_host",
  "worker_entrypoint": "libmy_awesome_arm.so",
  "download_url": null
}
```

#### Plugin Kinds

| Kind | Description | Examples |
|------|-------------|---------|
| `Native` | Compiled Rust/C++ shared library (.so) | image-batch, data-analytics |
| `Script` | Script-based worker (Python, Node, Shell) | data scrapers, converters |
| `AiModel` | ML model weights + inference engine | ai-whisper, LLaMA, Qwen |
| `WebHeadless` | Headless browser/WebView renderer | browser-cdp |

### 4. Build Your Binaries

```bash
# For Linux host (x86_64):
cargo build --release --target x86_64-unknown-linux-gnu

# For Android worker (ARM64):
# Requires Android NDK; set up the linker in .cargo/config.toml
cargo build --release --target aarch64-linux-android
```

### 5. Package & Checksum

```bash
# Create the release tarball
cd plugins/my-awesome-plugin/releases/v1.0.0/
tar -czf my-awesome-plugin.tar.gz manifest.json libmy_awesome.so

# Generate SHA-256 hash
sha256sum my-awesome-plugin.tar.gz
# Output: a1b2c3d4... my-awesome-plugin.tar.gz

# Update manifest.json with the sha256 and size_bytes values
```

### 6. Add Your Entry to `plugins.json`

Edit the root `plugins.json` file and add your plugin to the `"plugins"` array:

```json
{
  "id": "my-awesome-plugin",
  "name": "My Awesome Plugin",
  "version": "1.0.0",
  "adapter_id": "my_awesome.v1",
  "kind": "Native",
  "description": "A brief description of what your plugin does",
  "author": "Your Name",
  "license": "MIT",
  "size_bytes": 1500000,
  "sha256": "a1b2c3d4e5f6...",
  "download_url": "https://github.com/vijaygovindBiju/pocketbridge-plugins/releases/download/v1.0.0/my-awesome-plugin.tar.gz",
  "requirements": {
    "min_host_ram_mb": 256,
    "min_worker_ram_mb": 128,
    "min_worker_storage_mb": 20
  }
}
```

### 7. Write a README for Your Plugin

Create `plugins/my-awesome-plugin/README.md` explaining:
- What the plugin does
- Usage examples with `pocketbridge run`
- Any special configuration
- Performance benchmarks (if available)

### 8. Submit a Pull Request

```bash
git add -A
git commit -m "Add my-awesome-plugin v1.0.0"
git push origin main
# Then open a PR on GitHub
```

## Review Criteria

Your PR will be reviewed for:

1. **Security:** SHA-256 checksums match all binaries. No network calls to untrusted servers.
2. **Correctness:** Plugin manifest follows the schema. Binary runs on x86_64 Linux and ARM64 Android.
3. **Quality:** README is clear. The plugin does something useful and non-malicious.
4. **Size:** Model weights and binaries are reasonably sized. Multi-GB models should use chunked downloads.
5. **License:** A valid open-source license is specified.

## Rules

- **DO NOT** modify any file outside your `plugins/your-plugin-name/` directory (except adding one entry to `plugins.json`).
- **DO NOT** include source code that requires compilation by end users. All plugins must ship pre-compiled binaries.
- **DO NOT** bundle malware, cryptominers, or telemetry.
- **DO** test your plugin on real hardware (both Linux laptop and Android phone over USB) before submitting.
- **DO** specify accurate hardware requirements so pre-flight checks can protect users from OOM crashes.

## Questions?

Open an issue on this repository or on the [main PocketBridge repo](https://github.com/vijaygovindBiju/PocketBridge/issues).
