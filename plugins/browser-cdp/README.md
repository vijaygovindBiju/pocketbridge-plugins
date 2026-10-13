# Headless Chromium Tab Renderer

Offloads inactive Chrome/Firefox background tabs to your Android phone's WebView engine over USB, freeing laptop RAM for active work.

## Installation

```bash
pocketbridge plugin install browser-cdp
```

## How It Works

1. PocketBridge detects inactive browser tabs on your laptop via Chrome DevTools Protocol (CDP)
2. Background tabs are serialized and transferred to the phone over USB
3. The phone's Android WebView renders them in the background
4. When you switch back to a tab, it's restored from the phone instantly

This reclaims **hundreds of MB** of laptop RAM per offloaded tab.

## Usage

```bash
# Offload background tabs (auto-detects Chrome/Firefox)
pocketbridge run -a browser_cdp --hybrid

# Specify maximum tabs to offload
pocketbridge run -a browser_cdp --hybrid --max-tabs 10
```

## Hardware Requirements

- **Host RAM:** 512 MB minimum
- **Worker RAM:** 512 MB minimum
- **Worker Storage:** 50 MB minimum
- **Browser:** Chrome 90+ or Firefox 100+ with remote debugging enabled

## License

Apache-2.0
