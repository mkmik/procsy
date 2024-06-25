# procsy
minimal dependencies process manager + reverse proxy with cuda env var support.

## Install

Just copy the `procsy` script to a machines.

## Requirements

Uses `caddy` if locally installed; otherwise it tries to download caddy.

Currently it only downloads caddy for linux-x86_64. If you run `procsy` on another arch you must install caddy yourself.

## Usage

This starts 4 vllm instances each using 2 GPUs:

```bash
env \
    PROCSY_N=4 \
    PROCSY_PORT_TEMPLATE='--port $PORT' \
    PROCSY_CUDA_VISIBLE_DEVICES_SPAN=2 \
procsy python3 -m vllm.entrypoints.openai.api_server \
    --model mistralai/Mixtral-8x7B-Instruct-v0.1 \
    --tensor-parallel-size 2
```

