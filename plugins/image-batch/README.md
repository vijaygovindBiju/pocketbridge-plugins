# Image Batch Processor

High-throughput parallel image resizing, inversion, and box-blur filtering distributed across your Linux laptop and Android phone over USB.

## Installation

```bash
pocketbridge plugin install image-batch
```

## Usage

```bash
# Process 100 images with grayscale inversion (local only)
pocketbridge run -a image_batch --records 100 --local-only

# Process 500 images with box blur across laptop + phone
pocketbridge run -a image_batch --records 500 --hybrid --operation box_blur_3x3

# Process 1000 images with downscale, phone only
pocketbridge run -a image_batch --records 1000 --remote-only --operation downscale_half
```

## Supported Operations

| Operation | Description |
|-----------|-------------|
| `grayscale_invert` | Convert to grayscale and invert pixel values |
| `box_blur_3x3` | Apply 3x3 box blur kernel |
| `downscale_half` | Reduce image dimensions by 50% |

## Parameters

| Parameter | Flag | Default | Description |
|-----------|------|---------|-------------|
| Records | `--records` / `-r` | 50000 | Number of synthetic images to process |
| Width | `--width` | 256 | Image width in pixels |
| Height | `--height` | 256 | Image height in pixels |
| Operation | `--operation` | `grayscale_invert` | Image processing operation |
| Seed | `--seed` / `-s` | 0x123456789ABCDEF0 | PRNG seed for reproducible image generation |

## Hardware Requirements

- **Host RAM:** 256 MB minimum
- **Worker RAM:** 128 MB minimum
- **Worker Storage:** 15 MB minimum

## License

Apache-2.0
