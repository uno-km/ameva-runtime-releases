# AMEVA Runtime Public Binary Asset Registry

[![GitHub Release](https://img.shields.io/github/v/release/uno-km/ameva-runtime-releases?style=flat-square&color=0969da)](https://github.com/uno-km/ameva-runtime-releases/releases)
[![License](https://img.shields.io/badge/License-Apache_2.0-004499.svg?style=flat-square)](LICENSE)
<img src="https://img.shields.io/badge/Architecture-ARM64%20Bionic%20Android-green.svg" alt="ARM64 Android">
<img src="https://img.shields.io/badge/Vulkan-1.3%20Compute%20Accelerated-purple.svg?logo=vulkan&logoColor=white" alt="Vulkan 1.3">

Official public binary distribution and prebuilt native hardware acceleration registry for the **AMEVA Sovereign On-Device AI Ecosystem**.

This repository is dedicated exclusively to hosting cryptographically authenticated native binaries, compute pipelines (Vulkan/OpenCL/NPU/NEON), and distribution packages for Android Termux (`aarch64-linux-android`) and edge Linux devices.

---

## 1. Modality Acceleration Matrix & Binary Inventory

| Modality | Engine Binary | Package Archive | Size | Cryptographic SHA-256 (v2.7.2) | Target Acceleration |
| :--- | :--- | :--- | :---: | :--- | :--- |
| **Diffusion** | `sd-cli-vulkan` | `sd-cli-vulkan-android-arm64.tar.gz` | **39.5 MB** | `2d268255e84a37eca8e3ea047f79f944d23068108307f509cfd5c28484b05737` | Z-Image Turbo (6.0B DiT), Flash Attention, Layer Streaming, Mali-G78 & Adreno |
| **STT** | `whisper-cli-vulkan` | `whisper-cli-vulkan-android-arm64.tar.gz` | **15.2 MB** | `aeb32c3684369b71abac2646324f2fca51f9f3489f7ba575b70b5b72f133b75a` | Whisper Large-v3-Turbo, Bionic Zero-Collision Driver, Mobile Vulkan Compute |
| **LLM** | `llama-cli-vulkan` | `llama-cli-vulkan-android-arm64.tar.gz` | **125.3 MB** | `6775373f224ad6427225ed943c8bc38f065d14e6fe89dd56cf9b3bf2b7a56022` | GGUF LLM inference, 100% VRAM offload (`ngl=999`), ARM Cortex CPU-NEON fallback |
| **TTS** | `sherpa-ncnn-offline-tts` | `sherpa-ncnn-offline-tts-vulkan-arm64.tar.gz` | **3.9 MB** | `c112a4de96b31cee2d92b610fcc350ba4c2b2331eee25f3f86520b200992e8d5` | Sherpa-NCNN Neural Voice Synthesis, Vulkan HiFi-GAN On-chip Tiled Vocoder |
| **BitNet** | `termux-bitnet-cli` | `termux-bitnet-v1.4.0-android-aarch64.tar.gz` | **107 KB** | `6466c1d22917612a8c6b7bde29cce095c88fd0c736fe3e23c218695f5c675ea2` | Microsoft BitNet b1.58 (2B-4T i2_s), 1.58-bit Ternary Vulkan Acceleration |
| **Vision** | `termux-vision-cli` | `termux-vision-vulkan-android-arm64.tar.gz` | **14.9 MB** | `f190fb7f25d94c990b02e986afabca5897cbdf835906ff17a74d180f0ebd9da5` | Multimodal VLM (CLIP / MobileVLM / LLaVA) On-Device Vision Engine |

---

## 2. 1-Click Automated Installation Manual

All native engine bundles are designed for zero-configuration, tokenless deployment on any Android device running Termux ARM64.

### Option A: Python Orchestration Runtime (PIP)
```bash
# Direct install from public release wheel (No GitHub token required)
pip install --upgrade "https://github.com/uno-km/ameva-runtime-releases/releases/latest/download/ameva_runtime-2.7.3-py3-none-any.whl"

# Or standard PyPI install
pip install ameva-runtime
```

### Option B: Node.js & TypeScript Runtime (NPM)
```bash
# Install NPM package
npm install @ameva/runtime

# Or global CLI access
npm install -g @ameva/runtime
```

### Step 2: 1-Click Engine Auto-Provisioning
The `NativeAssetManager` inside `ameva` automatically downloads the latest prebuilt binaries from this registry, validates their SHA-256 checksums, and establishes atomic symlinks in `$PREFIX/bin`:

```bash
# Python CLI: Provision all hardware engines at once
ameva install --all

# Or NPM CLI: Provision via npx
npx ameva install --all

# Or provision a specific modality individually
ameva install --modality diffusion   # Installs sd-cli-vulkan -> $PREFIX/bin/sd-cli
npx ameva install --modality stt     # Installs whisper-cli-vulkan -> $PREFIX/bin/whisper-cli
ameva install --modality llm         # Installs llama-cli-vulkan -> $PREFIX/bin/llama-cli
ameva install --modality tts         # Installs sherpa-ncnn-offline-tts
npx ameva install --modality bitnet  # Installs termux-bitnet-cli
```

### Step 3: Run 12-Stage Hardware Diagnostic
```bash
# Python CLI
ameva doctor

# NPM CLI
npx ameva doctor
```
Verifies silicon topology, CPU core affinity, GPU kernel nodes (`/dev/kgsl-3d0`, `/dev/mali0`), Vulkan ICD loader chain, and asynchronous compute queue capabilities.

---

## 3. Direct Binary Execution & CLI Guide

Once provisioned, all binaries are immediately accessible from your shell:

### Stable Diffusion (Z-Image Turbo 6.0B DiT)
```bash
# 8-step photorealistic DiT generation with Flash Attention & Layer Streaming
sd-cli -m ~/.cache/termux-diffusion/models/z_image_turbo-Q2_K.gguf \
       --llm ~/.cache/termux-diffusion/models/Qwen3-4B-Instruct-2507-Q2_K.gguf \
       --vae ~/.cache/termux-diffusion/models/taef1.gguf \
       -p "a cybernetic tiger with glowing stripes prowling neon cyberpunk alley" \
       -s 8 -W 512 -H 512 --diffusion-fa --stream-layers -o output.png
```

### Speech-to-Text (Whisper Large-v3-Turbo)
```bash
whisper-cli -m ~/.cache/termux-stt/models/ggml-large-v3-turbo.bin \
            -f audio.wav -l ko -t 4 --beam-size 1
```

### Large Language Model (GGUF Vulkan Offload)
```bash
llama-cli -m ~/.cache/termux-llamacpp/models/qwen2.5-0.5b-instruct-q4_k_m.gguf \
          -p "Explain the physics of semiconductor lithography in 2 sentences." \
          -ngl 999 -t 4
```

---

## 4. Supply Chain Security & Zero-Silent-Fallback Protocol

Under the **Zero-Silent-Fallback** engineering standard:
1. **Cryptographic Verification**: Every downloaded archive is matched against its pinned SHA-256 before extraction. Any hash mismatch or network truncation aborts immediately.
2. **ABI & Architecture Safety**: Binary headers are validated for `ELF64`, little-endian, and `EM_AARCH64` (183) before being linked to the system path.
3. **Atomic Deployment & Manifesting**: Provisioned engines are isolated under `~/.local/share/ameva/releases/<modality>/<release_id>/` and tracked via authenticated `manifest.json`.

---

## 5. Two-Track Architecture Alignment

```
[Local Ground Truth / Offline Backup]
├── dev/ameva/ameva-runtime/releases/ (Consolidated 6-Modality SOTA Binaries)
└── dev/termux/termux-*/releases/     (Modular Domain Engine Binaries)
                     │
                     ▼ Synchronized Via Git & Release Pipelines
[Remote Distribution Clouds (Public)]
├── uno-km/ameva-runtime-releases    (Public Unified Binary Hub: Releases v2.7.2)
└── uno-km/termux-* (diffusion, stt) (Domain-Specific Public GitHub Releases)
```

---

## License

All native binaries, compute shaders, and precompiled runtime assets in this registry are distributed under the [Apache-2.0 License](LICENSE).
Copyright (c) 2026 Eunho Kim ([@uno-km](https://github.com/uno-km)) and the AMEVA Foundation.
