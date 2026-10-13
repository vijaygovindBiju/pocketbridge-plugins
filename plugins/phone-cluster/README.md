# Multi-Phone Cluster Orchestrator (`phone-cluster`)

High-throughput multi-device USB cluster orchestrator for PocketBridge. Connect 2, 4, 8, or more Android phones over USB to form a high-performance local mobile supercomputing cluster.

## Architecture

```
                       ┌─────────────────────────┐
                       │   PocketBridge Host     │
                       │   (Linux / Intel CPU)   │
                       └────────────┬────────────┘
                                    │
          ┌─────────────────────────┼─────────────────────────┐
          │ USB Port 1              │ USB Port 2              │ USB Port 3
          ▼                         ▼                         ▼
   127.0.0.1:9876            127.0.0.1:9877            127.0.0.1:9878
┌────────────────────┐    ┌────────────────────┐    ┌────────────────────┐
│   Phone #1         │    │   Phone #2         │    │   Phone #3         │
│ Snapdragon 8 Gen 3 │    │ Dimensity 9300     │    │ Tensor G4          │
│ (8 cores, 16GB)    │    │ (8 cores, 12GB)    │    │ (8 cores, 12GB)    │
└────────────────────┘    └────────────────────┘    └────────────────────┘
```

## Features

- **Automated Discovery**: Scans ADB for all authorized USB devices simultaneously.
- **Port Allocation**: Maps each phone to dedicated loopback ports (`127.0.0.1:9876`, `9877`, `9878`...).
- **Work-Stealing Queue**: Fast phones process more chunks; slow phones never stall the pipeline.
- **Hot-Unplug Failover**: If a cable is accidentally disconnected mid-job, pending chunks are automatically reclaimed and executed by remaining healthy phones.
- **Unified Telemetry**: Aggregates battery % and thermal statuses across all phones in real time.

## Installation

```bash
pocketbridge plugin install phone-cluster
```

Or install all catalog plugins:
```bash
pocketbridge plugin install all
```

## Usage

Run cluster orchestration benchmark:
```bash
pocketbridge run -a cluster --records 1000000
```

Execute distributed fleet across all phones:
```bash
pocketbridge fleet -a chunk_transform -n 500000 -c 16
```
