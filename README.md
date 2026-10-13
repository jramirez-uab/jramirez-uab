# Jorge Ramírez — JRamirez-UAB

Linux system administrator at the **DCC of the Universitat Autònoma de Barcelona (UAB)**.
I run local LLMs for one job: a **Linux system-administration agent** (opencode with tool calling) that our IT team uses **in Spanish**,
on hardware we already have — from a 12 GB gaming card to a 48 GB workstation GPU.

Everything here is quantized, served and tested for that use case, with **vLLM** (continuous batching, several sessions at once) as the serving engine.

## What I publish

| GPU memory | Models | Formats |
|---|---|---|
| **12–16 GB** | [NeoHorse-1-9B](https://huggingface.co/JRamirez-UAB/NeoHorse-1-9B-EXL3-16GB-12GB) · [Ornith-1.5-9B](https://huggingface.co/JRamirez-UAB/Ornith-1.5-9B-EXL3-16GB-12GB) | EXL3, 262k context on 16 GB |
| **24 GB** (RTX 3090) | Qwen3.8-27B · Swift 1.5 27B · Qwopus3.8-27B-Flash-V2 · Qwen3.8-27B-pi | EXL3 3.85/3.70, GPTQ W4A16 (INT4/INT8 embeddings), up to 222k context with MTP |
| **24 GB**, MoE | Qwen3.6-35B-A3B · Qwopus3.6-35B-A3B-Coder · Ornith-1.5-35B-A3B | EXL3 with our vLLM plugin for MoE experts and vision |
| **48 GB** (RTX PRO 5000, L40S, A40) | [Swift 1.5 Qwen3.8-Flash-Next](https://huggingface.co/JRamirez-UAB/Swift-1.5-Qwen3.8-Flash-Next-EXL3-2.25bpw-48GB) · [Qwen3.8-Flash-Next](https://huggingface.co/JRamirez-UAB/Qwen3.8-Flash-Next-EXL3-2.25bpw-48GB) (125B MoE) | EXL3 2.25 bpw: 4 sessions × 262k, vision, MTP on one 48 GB GPU |
| Other | [Clef 27B](https://huggingface.co/JRamirez-UAB/clef-GPTQ-W4A16-g64-embed-int8) (decision model) · [DFlash 2 draft](https://huggingface.co/JRamirez-UAB/Qwen3.8-27B-DFlash2-W4A16) | GPTQ W4A16 |

## How I test

Every card uses the same template and reports the same checks, measured on the quantized model itself:
- **KL divergence against BF16** on turboderp's public trace *and* on our own Spanish sysadmin trace.
- **Tool calls with the real opencode request** (its system prompt and tools, captured and replayed), with and without reasoning.
- **Agent tasks with real command execution** (`opencode run`), plus a set of hard tasks (Slurm, FreeIPA, systemd, nginx, nftables, ZFS, logs).
- **Long context** (needle-in-a-haystack up to 245k tokens), vision, concurrency and the **real free GPU memory at peak** (≥100 MiB).
- **Repetition loops by temperature** on long answers — the reason some cards insist on a specific temperature.

Some things I learned along the way are written in the cards: KLD alone does not predict agent quality; models "translate" output file names
that are system concepts (backticks fix it); and a model that invents CLI options gets them right when it can read the tool's real `--help`.

## Feedback

Installation reports — **working or failed** — are very welcome in each model's Community tab: GPU, driver, vLLM version, command line and result.
