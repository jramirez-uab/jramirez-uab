# Jorge Ramírez Espejo

Linux system administrator at the **DCC of the Universitat Autònoma de Barcelona (UAB)**. *Aprender enseñando.*

I run local LLMs for one job: a **Linux system-administration agent** (opencode with tool calling) that our IT team uses **in Spanish**,
on hardware we already have, from a 12 GB gaming card to a 48 GB workstation GPU. Everything below comes out of that work:
models quantized, served and tested for that use case, with **vLLM** (continuous batching, several sessions at once) as the serving engine.

## Open source

| Project | What it is |
|---|---|
| [vllm-quantpack](https://github.com/jramirez-uab/vllm-quantpack) | One vLLM plugin for EXL3 checkpoints and the model-family fixes they need, with no patch to vLLM. On [PyPI](https://pypi.org/project/vllm-quantpack/) (experimental, early stage). |
| [service-ple](https://github.com/jramirez-uab/service-ple) | Serve a PLE embedding table over the network: one copy for many inference servers (experimental, early stage). |
| [OrcaSAQ2-kernel](https://github.com/jramirez-uab/OrcaSAQ2-kernel) | Serving kernels, vLLM plugin and launch presets for OrcaSAQ2. |
| [verbatim-proxy](https://github.com/jramirez-uab/verbatim-proxy) | OpenAI-compatible proxy that keeps file names verbatim when a coding agent works in a language other than English. |

### Contributions to vLLM
- [#67100](https://github.com/vllm-project/vllm/pull/67100): store TurboQuant k8v4 FP8 keys as real E4M3 below sm_89. On Ampere the keys were clipped at ±1.875 and the model lost the context (needles 0/8 at 60k, 8/8 with the fix).
- [#67102](https://github.com/vllm-project/vllm/pull/67102): INT4 KV-cache read kernel, zero-point sum taken on the rounded P. Error against an fp64 reference grew from 3.8 % at 4k tokens to 28 % at 229k; with the fix it stays at 0.4 %.
- [#67103](https://github.com/vllm-project/vllm/issues/67103): RFC for an int8-key / int4-value KV-cache dtype.
- [Measurements on #40792](https://github.com/vllm-project/vllm/pull/40792#issuecomment-6807268753): k8v4 decode attention on Ampere and Blackwell.

## Models on Hugging Face

[JRamirez-UAB](https://huggingface.co/JRamirez-UAB): 36 public models, grouped in [collections](https://huggingface.co/JRamirez-UAB/collections) by GPU memory. EXL3 unless stated.

| GPU memory | Models |
|---|---|
| **8–16 GB** | [Kestrel 4B Coder](https://huggingface.co/JRamirez-UAB/Kestrel-4B-Coder-EXL3-6.00bpw-8-16GB) · [Spark X2.5 4B](https://huggingface.co/JRamirez-UAB/Spark-X2.5-4B-EXL3-6.00bpw-8-16GB) · [NeoHorse-1 9B](https://huggingface.co/JRamirez-UAB/NeoHorse-1-9B-EXL3-16GB-12GB) · [Ornith-1.5 9B](https://huggingface.co/JRamirez-UAB/Ornith-1.5-9B-EXL3-16GB-12GB) (262k context on 16 GB) |
| **16–24 GB** | [Groot 19B-A3B](https://huggingface.co/JRamirez-UAB/Groot-19B-A3B-EXL3-4.00bpw-16-24GB) · [ZwZ2 27B 2.25 bpw](https://huggingface.co/JRamirez-UAB/ZwZ2-27B-EXL3-2.25bpw-16GB) · [ZwZ2 27B 4.0 bpw](https://huggingface.co/JRamirez-UAB/ZwZ2-27B-EXL3-4.0bpw-24GB) |
| **24 GB** (RTX 3090), dense 27B | [Qwen3.8-27B](https://huggingface.co/JRamirez-UAB/Qwen3.8-27B-EXL3-3.85bpw-24GB) · [Swift 1.5](https://huggingface.co/JRamirez-UAB/Swift-1.5-Qwen3.8-27B-EXL3-3.85bpw-24GB) · [Qwopus3.8-27B-Flash-V2](https://huggingface.co/JRamirez-UAB/Qwopus3.8-27B-Flash-V2-EXL3-3.85bpw-24GB) · [Qwopus3.8-27B-Coder-Flash](https://huggingface.co/JRamirez-UAB/Qwopus3.8-27B-Coder-Flash-EXL3-3.50bpw-24GB) · [Qwen3.8-27B-pi](https://huggingface.co/JRamirez-UAB/Qwen3.8-27B-pi-EXL3-3.85bpw-24GB) · [Qwen3.8-27B-Coder390](https://huggingface.co/JRamirez-UAB/Qwen3.8-27B-Coder390-EXL3-3.50bpw-24GB) · [Swift 1.5 heretic](https://huggingface.co/JRamirez-UAB/Swift-1.5-Qwen3.8-27B-heretic-EXL3-3.85bpw-24GB) (uncensored). Also GPTQ W4A16 with INT4/INT8 embeddings, up to 222k context with MTP |
| **24 GB**, MoE | [Qwen3.6-35B-A3B](https://huggingface.co/JRamirez-UAB/Qwen3.6-35B-A3B-EXL3-4.00bpw-24GB) · [Qwopus3.6-35B-A3B-Coder](https://huggingface.co/JRamirez-UAB/Qwopus3.6-35B-A3B-Coder-EXL3-4.00bpw-24GB) · [Ornith-1.5-35B-A3B](https://huggingface.co/JRamirez-UAB/Ornith-1.5-35B-A3B-EXL3-4.00bpw-24GB) · [Nemotron-3.5-Lightning 30B-A3B](https://huggingface.co/JRamirez-UAB/Nemotron-3.5-Lightning-30B-A3B-EXL3-3.25bpw-24GB) · [Flint 40B-A2.5B](https://huggingface.co/JRamirez-UAB/Flint-40B-A2.5B-EXL3-3.88bpw-24-48GB) (24–48 GB). Run with our vLLM plugin for MoE experts and vision |
| **48 GB** (RTX PRO 5000, L40S, A40) | [Swift 1.5 Qwen3.8-Flash-Next](https://huggingface.co/JRamirez-UAB/Swift-1.5-Qwen3.8-Flash-Next-EXL3-2.25bpw-48GB) · [Qwen3.8-Flash-Next](https://huggingface.co/JRamirez-UAB/Qwen3.8-Flash-Next-EXL3-2.25bpw-48GB) (125B MoE, EXL3 2.25 bpw: 4 sessions × 262k, vision, MTP on one 48 GB GPU) |
| **96 GB** | [Swift 1.5 Flash-Next 3.75 bpw](https://huggingface.co/JRamirez-UAB/Swift-1.5-Qwen3.8-Flash-Next-EXL3-3.75bpw-96GB) · [Nemotron-3.5-Super-VL 120B-A12B](https://huggingface.co/JRamirez-UAB/Nemotron-3.5-Super-VL-120B-A12B-EXL3-2.25bpw-VL-48-96GB) (48–96 GB) |
| Other | [Clef 27B](https://huggingface.co/JRamirez-UAB/clef-GPTQ-W4A16-g64-embed-int8) (decision model) · [DFlash 2 draft](https://huggingface.co/JRamirez-UAB/Qwen3.8-27B-DFlash2-W4A16) · [DSpine vLLM quants](https://huggingface.co/JRamirez-UAB/Qwen3.8-27B-DSpine-vLLM-quants) · [HybridAgent 8B](https://huggingface.co/JRamirez-UAB/hybridagent-8b-exl3-vllm-experimental) (experimental) |

## How I test

Every card uses the same template and reports the same checks, measured on the quantized model itself:
- **KL divergence against BF16** on turboderp's public trace *and* on our own Spanish sysadmin trace.
- **Tool calls with the real opencode request** (its system prompt and tools, captured and replayed), with and without reasoning.
- **Agent tasks with real command execution** (`opencode run`), plus a set of hard tasks (Slurm, FreeIPA, systemd, nginx, nftables, ZFS, logs).
- **Long context** (needle-in-a-haystack up to 245k tokens), vision, concurrency and the **real free GPU memory at peak** (≥100 MiB).
- **Repetition loops by temperature** on long answers, the reason some cards insist on a specific temperature.

Some things I learned along the way are written in the cards: KLD alone does not predict agent quality; models "translate" output file names
that are system concepts (backticks fix it); and a model that invents CLI options gets them right when it can read the tool's real `--help`.

## Feedback

Installation reports, **working or failed**, are very welcome in each model's Community tab: GPU, driver, vLLM version, command line and result.
