# procsy
minimal dependencies process manager + reverse proxy with cuda env var support.

It runs N instances of a command in parallel and kills them all when you kill/interrupt `procsy`.

The main differences between this and the other process managers out there are:

1. mostly-purely-bash (requires `curl` to download `caddy` if not already installed)
2. no configuration files, fits in a one-liner to copy paste on slack with your colleagues
3. integrates a HTTP reverse with the process manager so you don't have to configure both
4. supports commands that cannot be configured with a `PORT` env var but require flags.
5. handles the `CUDA_VISIBLE_DEVICES` variable

## Install

Just copy the `procsy` script to a machines.

## Requirements

Uses `caddy` if locally installed; otherwise it tries to download caddy (using `curl`).

Currently it only downloads caddy for linux-x86_64. If you run `procsy` on another arch you must install caddy yourself.

## Usage

Run `procsy` or `procsy --help` for usage docs


## Example

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

