# AMEVA Runtime Binary Asset Registry

Official public binary distribution and prebuilt native accelerator registry for **AMEVA Runtime**.

This repository is dedicated exclusively to hosting cryptographically authenticated native binaries, hardware acceleration compute pipelines (Vulkan/OpenCL/NPU/NEON), and distribution packages for Android Termux (ARM64/Bionic) and edge Linux devices.

---

## Modality Native Assets

| Modality | Engine Binary | Target Architecture | Hardware Acceleration |
| :--- | :--- | :--- | :--- |
| **Diffusion** | `sd-cli-vulkan` | `android-arm64` / Bionic | Adreno 6xx/7xx/8xx, Mali-G78/Valhall, CPU-NEON |
| **STT** | `whisper-cli-vulkan` | `android-arm64` / Bionic | Mobile Vulkan Compute & Universal Driver |
| **LLM** | `llama-cli-vulkan` | `android-arm64` / Bionic | Vulkan MatMul FP16 / CPU DotProd |
| **TTS** | `sherpa-ncnn-offline-tts` | `android-arm64` / Bionic | NCNN Vulkan Neural Vocoder |
| **BitNet** | `termux-bitnet-cli` | `android-arm64` / Bionic | 1.58-bit Ternary Quantized Acceleration |

---

## Cryptographic Integrity Guarantee

All distribution archives hosted in GitHub Releases are accompanied by explicit SHA-256 checksums (`.sha256`) and are automatically verified by the `NativeAssetManager` inside `ameva-runtime` before any file extraction or execution occurs under our strict **Zero-Silent-Fallback** protocol.

---

## License

All native binaries and runtime assets distributed through this registry are licensed under the [Apache-2.0 License](LICENSE).
