# Data Analytics Engine

Structured CSV/TSV table reduction, sum, min/max, and numerical filtering distributed across your Linux laptop and Android phone over USB.

## Installation

```bash
pocketbridge plugin install data-analytics
```

## Usage

```bash
# Process 50,000 records locally
pocketbridge run -a data_analytics --records 50000 --local-only

# Process 1 million records across laptop + phone
pocketbridge run -a data_analytics --records 1000000 --hybrid
```

## Parameters

| Parameter | Flag | Default | Description |
|-----------|------|---------|-------------|
| Records | `--records` / `-r` | 50000 | Number of synthetic data records |
| Seed | `--seed` / `-s` | 0x123456789ABCDEF0 | PRNG seed for reproducible data |

## Hardware Requirements

- **Host RAM:** 128 MB minimum
- **Worker RAM:** 64 MB minimum
- **Worker Storage:** 10 MB minimum

## License

Apache-2.0
