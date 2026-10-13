# LLaMA-3 Transformer Engine (`llm-llama`)

Distributed pipeline parallel LLM inference engine for PocketBridge.

## Features
- **Tokens/Sec Benchmarking**: Measures tokens per second across Snapdragon 8 Elite NPU and Intel CPU.
- **Pipeline Parallel Layers**: Dynamically divides 32 transformer layers across host and worker phone.
- **Speculative Decoding**: Drafts fast tokens on mobile NPU, verifies in parallel on host.
- **Activation Streaming**: FP16 intermediate tensors stream over USB link (371 Mbps).

## Usage
```bash
pocketbridge plugin install llm-llama
pocketbridge run -a llm_pipeline --remote-only
pocketbridge run -a llm_pipeline --hybrid
```
