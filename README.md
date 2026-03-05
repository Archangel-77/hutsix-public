# Hutsix (Hut6)

![Windows](https://img.shields.io/badge/Platform-Windows%2010%2F11-blue)
![Python](https://img.shields.io/badge/Python-3.10-3776AB)
![CUDA](https://img.shields.io/badge/Acceleration-CUDA%20Supported-76B900)
![Status](https://img.shields.io/badge/Status-Actively%20Developed-brightgreen)
![License](https://img.shields.io/badge/License-Proprietary-red)

Hutsix (Hut6) is a Windows desktop automation platform built with Python and PySide6.
It lets users automate repetitive keyboard/mouse workflows through a profile-based visual system, without requiring custom scripts.

This repository is the public showcase for the project.

## Get Hutsix and Join Community

| Download | Community |
|---|---|
| [Get Hutsix on Gumroad](https://panpanageas.gumroad.com/l/rrstan) | [Join Hutsix Discord](https://discord.gg/Sr5WUmeY) |

## Table of Contents

- [Demo](#demo)
- [Problem -> Solution](#problem---solution)
- [What Hutsix Does](#what-hutsix-does)
- [Core Features](#core-features)
- [Example Use Cases](#example-use-cases)
- [Architecture Overview](#architecture-overview)
- [Performance and Acceleration](#performance-and-acceleration)
- [System Requirements](#system-requirements)
- [Quick Start](#quick-start)
- [Troubleshooting](#troubleshooting)
- [Known Limitations](#known-limitations)
- [Privacy](#privacy)
- [Comparison Snapshot](#comparison-snapshot)
- [Roadmap](#roadmap)
- [FAQ](#faq)
- [Responsible Use](#responsible-use)
- [Tech Stack](#tech-stack)
- [Project Status](#project-status)
- [Versioning and Changelog](#versioning-and-changelog)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)

## Demo

Live demos, setup walkthroughs, and feature previews are available on the official channel:

- YouTube: [@Hutsix](https://www.youtube.com/@Hutsix)
- Product walkthroughs and feature demos are published on the channel above

### Feature GIF Preview

- Short feature demo clips are published on YouTube Shorts and videos in the channel above.

## Problem -> Solution

Many users repeat the same input sequences across desktop apps and games.
Hutsix solves this by combining reliable trigger detection (hotkeys, pixels, image, OCR text) with controlled action playback, diagnostics, and profile management in one desktop app.

## What Hutsix Does

Hutsix uses **profiles** made of **bindings**:

- A **trigger** detects an event (hotkey, pixel color, image match, text match).
- An **action** runs keyboard/mouse sequences.

When a profile is active, a watchdog engine continuously evaluates bindings and executes actions only when configured conditions are satisfied.

## Core Features

- Profile-based automation with import/export and reuse
- Global hotkeys and shared trigger handling
- Pixel detection with HDR/SDR-aware behavior
- Image/template matching with configurable confidence
- OCR-based text detection in selected screen regions
- Advanced YOLOX dataset creation workflows
- Advanced YOLOX training modules for custom detection models
- GPU-accelerated AI workflows (CUDA-enabled environments)
- Keyboard and mouse sequence recording/replay
- Focus guard (restrict playback to a target executable)
- Cooldown controls (global + per-binding)
- Action Monitor and per-binding diagnostics

## Example Use Cases

- Repetitive desktop productivity actions
- Input helper routines for full-screen apps
- Fast profile switching across workflows
- Sharing automation profiles with other users

## Architecture Overview

```text
+-------------------+       +--------------------+
|    PySide6 UI     | <-->  | Profile/Settings   |
+-------------------+       +--------------------+
          |                           |
          v                           v
+-------------------+       +--------------------+
| Trigger Evaluator | ----> |  Watchdog Scheduler|
| (Hotkey/Pixel/Img)|       +--------------------+
+-------------------+                   |
          |                             v
          |                   +--------------------+
          +-----------------> |  Action Executor   |
                              | (Keyboard/Mouse)   |
                              +--------------------+
                                          |
                                          v
                              +--------------------+
                              | Diagnostics/Logs   |
                              +--------------------+
```

### Main Components

- `UI Layer`: profile editing, trigger/action configuration, monitoring
- `Trigger Engine`: hotkey, pixel, image, and OCR evaluation
- `Watchdog`: schedule and condition loop with cooldown enforcement
- `Action Executor`: deterministic keyboard/mouse playback
- `Diagnostics`: event status, failure reasons, and monitoring timeline

## Performance and Acceleration

Hutsix supports CPU and GPU execution paths, depending on the module and environment.

- CUDA-enabled acceleration for supported AI/vision workloads
- GPU or CPU execution modes for OCR and model-based detection pipelines
- Efficient desktop capture + trigger evaluation loop for responsive automation
- Fallback to CPU paths when GPU acceleration is unavailable

## System Requirements

### Validated Setup (Author Workstation)

- OS: Windows 11 Pro (Build 26200)
- CPU: AMD Ryzen 9 5900X (12 cores / 24 threads)
- RAM: 32 GB
- GPU: NVIDIA GeForce RTX 4060 Ti (16 GB VRAM)
- NVIDIA Driver: 595.71
- CUDA (driver runtime): 13.2
- Python runtime: 3.10
- Key ML/CV packages:
  - `torch==2.5.1+cu121`, `torchvision==0.20.1+cu121`, `torchaudio==2.5.1+cu121`
  - `yolox==0.3.0`
  - `onnxruntime-gpu==1.23.2`
  - `easyocr==1.7.2`

### Practical Baseline

- OS: Windows 10/11 (64-bit)
- CPU: 6+ logical cores
- RAM: 16 GB baseline for core automation features
- RAM: 32 GB required for AI modules (YOLOX/OCR/training workflows)
- GPU: NVIDIA GPU with CUDA support for best YOLOX/OCR performance
- VRAM: 8 GB+ recommended for smoother training/inference
- Storage: SSD with 10 GB+ free space (more for datasets/models)

### Compatibility Notes

- CUDA acceleration requires compatible NVIDIA drivers and CUDA-enabled dependencies.
- If CUDA is unavailable, supported features can run in CPU mode with reduced performance.
- Actual performance depends on profile complexity, capture resolution, and active modules (OCR/inference/training).

## Quick Start

1. Install and launch a Hutsix release build (`Hutsix.exe` / MSI package).
2. Create or import a profile.
3. Add a trigger and attach an action sequence.
4. Activate the profile.
5. Use Action Monitor/Diagnostics to validate behavior.

### Distribution Notes

- Release packaging is produced as MSI + ZIP artifacts in the project release pipeline.
- Installer workflow includes bundled runtime dependencies required for `cv2`/CUDA-enabled paths on supported systems.
- If you are running a direct executable build, keep bundled runtime files beside `Hutsix.exe`.

## Sample Binding (Concept)

```json
{
  "binding_name": "AutoPotion",
  "trigger": {
    "type": "pixel",
    "x": 1420,
    "y": 960,
    "rgb": [210, 35, 40],
    "tolerance": 8
  },
  "action": {
    "type": "keyboard_sequence",
    "steps": [
      { "key": "5", "event": "down" },
      { "key": "5", "event": "up", "delay_ms": 25 }
    ]
  },
  "cooldown_ms": 1200,
  "target_exe": "example.exe"
}
```

## Before / After Workflow

- Before Hutsix: manually repeat hotkeys and mouse sequences, troubleshoot failures by guesswork.
- After Hutsix: triggers and actions are profile-driven, repeatable, and diagnosable through Action Monitor.

## Troubleshooting

- `ImportError: DLL load failed while importing cv2`
  - Ensure runtime DLLs are next to `Hutsix.exe` (`cudart64_12.dll`, `cublas64_12.dll`, `opencv_core4120.dll`).
- AI modules are slow
  - Verify NVIDIA drivers and CUDA-enabled dependencies are installed.
  - Confirm GPU path is available; CPU fallback is supported but slower.
- Trigger not firing
  - Check focus guard target executable.
  - Review cooldown settings and per-binding diagnostics.

## Known Limitations

- Performance can vary significantly by resolution, active bindings, and OCR/model workload.
- GPU acceleration depends on compatible NVIDIA hardware/drivers and runtime dependencies.
- OCR/image triggers may require tuning thresholds per game/app and display conditions.

## Privacy

- Hutsix is a local desktop application.
- Profiles, logs, and runtime data are stored locally on your system.
- Licensing checks may contact the configured licensing provider during activation/verification.

## Comparison Snapshot

| Capability | Hutsix | Basic Macro Tools |
|---|---|---|
| Visual profile system | Yes | Tool-dependent |
| Pixel/Image/OCR triggers | Yes | Often hotkey-only or partial |
| Diagnostics per binding | Yes | Commonly limited |
| Focus guard + cooldown model | Yes | Tool-dependent |
| Import/export profile workflows | Yes | Tool-dependent |

## Roadmap

- Better profile packaging and sharing flows
- More trigger tuning controls and presets
- Improved OCR and model inference tuning options
- Extended diagnostics timeline and replay trace tooling
- Additional UI quality-of-life improvements

## FAQ

### Does Hutsix inject into game memory?
No. It is designed around input simulation and screen/foreground detection.

### Can automation violate third-party terms?
Yes. Some apps/services prohibit automation.

### Is coding required to use Hutsix?
No. Typical usage is fully UI-driven.

### Can I share my profiles?
Yes. Profiles are intended to be reusable and shareable.

## Responsible Use

Hutsix is a general-purpose automation tool.
Using automation with third-party software (including games/online services) may violate their Terms of Service and can lead to penalties (including account restrictions or bans).

Users are solely responsible for how they use the software.

## Tech Stack

- Python
- PySide6
- OpenCV
- Optional OCR / AI-assisted detection modules
- YOLOX-based dataset tooling and training pipeline modules
- CUDA-enabled GPU acceleration

## Project Status

Actively developed. Ongoing work focuses on reliability, diagnostics, UX, and feature depth.

## Versioning and Changelog

- Current public showcase update: `2026-03-05`
- Release updates and feature changes are announced through release artifacts and official Hutsix channels.

## Contributing

This is a proprietary project showcase.
Code contributions are not open in this repository.

Feedback, bug reports, and feature suggestions are welcome through:

- Discord: [https://discord.gg/Sr5WUmeY](https://discord.gg/Sr5WUmeY)
- YouTube: [https://www.youtube.com/@Hutsix](https://www.youtube.com/@Hutsix)

## Credits

Built with major third-party technologies including:

- PySide6
- OpenCV
- PyTorch
- ONNX Runtime
- EasyOCR
- YOLOX

## License

Hutsix is proprietary software © 2025–2026 Panagiotis Panageas.
Redistribution or modification is not permitted without written permission, except where required by third-party licenses.
See `LICENSE` and `THIRD_PARTY_LICENSES.md` in the main project repository for full terms.
