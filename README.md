# AI Image Transformer

[![CI](https://github.com/Kernelkaos/ai-image-transformer-for-mac-and-cuda/actions/workflows/ci.yml/badge.svg)](https://github.com/Kernelkaos/ai-image-transformer-for-mac-and-cuda/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Python](https://img.shields.io/badge/python-3.10--3.13-blue.svg)
![Platforms](https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20Windows-lightgrey.svg)

A local web app that transforms images with **Stable Diffusion XL (SDXL)** and **ControlNet Canny**. It extracts the edges of your input image and generates a new image from your text prompt while keeping the original structure.

Runs on **NVIDIA GPUs (CUDA)**, **Apple Silicon (MPS)** and, slowly, on **CPU**.

## Table of contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Project structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

## Features

- Image-to-image transformation guided by Canny edges (structure is preserved).
- Gradio web dashboard with prompt, negative prompt, steps, guidance scale, edge influence, Canny thresholds and seed.
- Automatic device selection: CUDA, then Apple Silicon (MPS), then CPU.
- Memory management: FP16, attention slicing, VAE tiling, optional CPU offload, and cache cleanup between runs.
- Optional xFormers attention on CUDA.
- Settings kept in a single YAML file.
- Binds to `127.0.0.1` by default, so it is not exposed to your network.

## Requirements

| Item | Requirement |
|---|---|
| Python | 3.10 to 3.13 (PyTorch does not yet publish wheels for newer versions) |
| Disk space | About 15 GB for the model downloads |
| GPU | NVIDIA (CUDA) or Apple Silicon recommended |
| Memory | 16 GB RAM or VRAM recommended for SDXL |
| Internet | Needed on first run to download the models from Hugging Face |

## Installation

```bash
git clone https://github.com/Kernelkaos/ai-image-transformer-for-mac-and-cuda.git
cd ai-image-transformer-for-mac-and-cuda

python3 -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

**NVIDIA / CUDA only (optional):** for faster, lower-memory attention, also run `pip install xformers`. It is not available on macOS.

## Usage

```bash
python app.py
```

Then open <http://127.0.0.1:7860>.

| Option | Default | Description |
|---|---|---|
| `--host` | `127.0.0.1` | Address to bind the server to |
| `--port` | `7860` | Port to listen on |
| `--share` | off | Create a public Gradio link |

> [!WARNING]
> `--share` makes the app reachable from the internet. Only use it if you understand the risk.

On the first generation the app downloads these models from Hugging Face (about 12 to 15 GB, cached afterwards):

- `stabilityai/stable-diffusion-xl-base-1.0`
- `diffusers/controlnet-canny-sdxl-1.0`
- `madebyollin/sdxl-vae-fp16-fix`

Please check the license of each model before using the results commercially.

## Configuration

All settings live in [`config/settings.yaml`](config/settings.yaml).

| Section | Key | Default | Purpose |
|---|---|---|---|
| `model_settings` | `sdxl_base_model` | `stabilityai/stable-diffusion-xl-base-1.0` | Base model |
| | `controlnet_canny_model` | `diffusers/controlnet-canny-sdxl-1.0` | ControlNet model |
| | `vae_model` | `madebyollin/sdxl-vae-fp16-fix` | VAE that avoids black images in FP16 |
| | `precision` | `fp16` | `fp16` or `fp32` |
| `inference_constraints` | `default_steps` | `30` | Denoising steps |
| | `default_guidance_scale` | `7.5` | Prompt adherence |
| | `controlnet_conditioning_scale` | `0.5` | Edge influence |
| `performance` | `enable_attention_slicing` | `true` | Lower memory use |
| | `enable_vae_tiling` | `true` | Larger images on small GPUs |
| | `enable_cpu_offload` | `false` | For GPUs with less than 8 GB (CUDA) |
| | `enable_xformers` | `true` | CUDA only, ignored elsewhere |

## Project structure

```text
.
├── app.py                 # Command-line entry point
├── config/
│   └── settings.yaml      # Model and performance settings
├── src/
│   ├── core/
│   │   ├── engine.py      # SDXL + ControlNet pipeline and Canny processing
│   │   └── memory.py      # Memory cleanup and usage reporting
│   └── ui/
│       └── dashboard.py   # Gradio interface
├── .github/               # CI workflow, issue and pull request templates
├── requirements.txt
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
└── LICENSE
```

## Troubleshooting

| Problem | Fix |
|---|---|
| `No matching distribution found for torch` | Use Python 3.10 to 3.13. |
| `xformers` fails to install | Skip it. It is only needed on NVIDIA GPUs. |
| Out of memory | Enable `enable_cpu_offload` (CUDA), lower the steps, or use a smaller input image. |
| Black images | Keep the FP16-fixed VAE from `settings.yaml`, or set `precision: fp32`. |
| Very slow generation | You are probably running on CPU. Check the "Hardware Status" box in the app. |

## Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) and follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Security

To report a vulnerability, see [SECURITY.md](SECURITY.md).

## License

Released under the [MIT License](LICENSE). The AI models are distributed under their own licenses.
