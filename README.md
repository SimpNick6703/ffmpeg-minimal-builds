# ffmpeg-minimal-builds

[![GitHub Release](https://img.shields.io/github/v/release/SimpNick6703/ffmpeg-minimal-builds?style=flat-square&color=06b6d4)](https://github.com/SimpNick6703/ffmpeg-minimal-builds/releases)
[![Build Status](https://img.shields.io/github/actions/workflow/status/SimpNick6703/ffmpeg-minimal-builds/build.yml?branch=main&style=flat-square&label=build)](https://github.com/SimpNick6703/ffmpeg-minimal-builds/actions/workflows/build.yml)
[![Downloads](https://img.shields.io/github/downloads/SimpNick6703/ffmpeg-minimal-builds/total?style=flat-square&color=8b5cf6)](https://github.com/SimpNick6703/ffmpeg-minimal-builds/releases)
[![License: GPL-3.0](https://img.shields.io/badge/License-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
[![Platform Support](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-emerald?style=flat-square)](#target-platform-matrix)
[![Used by Video-Compression](https://img.shields.io/badge/Used%20by-Video--Compression-f59e0b?style=flat-square&logo=github)](https://github.com/SimpNick6703/Video-Compression)

Automated multi-platform builds of minimal, static `ffmpeg` and `ffprobe` binaries tailored specifically for H.264 & HEVC video transcoding with hardware acceleration and audio passthrough.

These builds serve as the upstream binary provider bundled inside standalone executables for [Video-Compression](https://github.com/SimpNick6703/Video-Compression).

## Target Platform Matrix

| Platform | Arch | Hardware Acceleration | Software Codecs | Archive Name |
|:---|:---|:---|:---|:---|
| **Linux** | `x86_64` | NVENC (`cuda`, `scale_cuda`), VA-API (`scale_vaapi`) | `libx264`, `libx265` | `ffmpeg-minimal-linux-x86_64.tar.gz` |
| **Windows** | `x86_64` | NVENC (`cuda`, `scale_cuda`), AMD AMF (`vpp_amf`), Intel QSV (`libvpl`, `vpp_qsv`) | `libx264`, `libx265` | `ffmpeg-minimal-win-x86_64.zip` |
| **macOS** | `arm64` | Apple VideoToolbox (`scale_vt`) | `libx264`, `libx265` | `ffmpeg-minimal-macos-arm64.tar.gz` |
| **macOS** | `x86_64` | Apple VideoToolbox (`scale_vt`) | `libx264`, `libx265` | `ffmpeg-minimal-macos-x86_64.tar.gz` |

## Size Optimization

- Configured with `--disable-everything` and selective `--enable-*` flags.
- Symbol tables stripped (`strip`).
- Compressed with UPX (`upx --best`) on Linux.
- Packaged as clean stripped binaries on Windows to eliminate antivirus heuristic false positives.
- Total binary size: **~7–15 MB per archive** (down from ~140 MB+ full builds).

## Automation

- Checks for new stable FFmpeg release tags (`n*`) every Sunday at 12:00 PM IST (06:30 UTC).
- Automatically skips rebuilding if a GitHub Release for the current stable tag already exists.
- Publishes pre-built archives as GitHub Release assets tagged `ffmpeg-<tag>`.
- Each build job verifies that the required hardware filters exist in the built binary and fails otherwise.
- Releases are never overwritten. Running the workflow manually from `main` for an FFmpeg version that is already released publishes the next revision automatically (`ffmpeg-n9.0.2` → `ffmpeg-n9.0.2-r2` → `-r3` …). The optional `release_revision` input (e.g. `r5`) picks a specific revision instead; the run fails if that revision already exists.
- Manual runs from other branches build and verify the binaries but publish nothing.

## Used By

- [Video-Compression](https://github.com/SimpNick6703/Video-Compression) — Preset-based video compression tool targeting Discord file size limits, embedding these minimal binaries directly into zero-dependency standalone executables.

## License

The build scripts and resulting prebuilt `ffmpeg` and `ffprobe` binaries (compiled with `--enable-gpl --enable-version3 --enable-libx264 --enable-libx265`) are distributed under the **GPL-3.0-or-later** license. See [LICENSE](LICENSE) for details.

No proprietary code is compiled in: the builds do not use `--enable-nonfree`, CUDA filters are compiled with clang (`--enable-cuda-llvm`) instead of the NVIDIA CUDA toolkit, and GPU vendor runtimes (NVIDIA, AMD, Intel drivers) are only loaded from the system at run time.

### Corresponding Source

Every release archive contains:

- `BUILD_INFO.txt`: the exact source revision of each component (FFmpeg tag and commit, x264, x265, nv-codec-headers, AMF headers, and the MSYS2 package versions used on Windows), the workflow run that produced the binaries, and the full FFmpeg configuration.
- `licenses/`: the license texts of FFmpeg and of every component compiled into the binaries.

The source for each listed revision is available from the linked upstream repositories. The build recipe is [`.github/workflows/build.yml`](.github/workflows/build.yml) at the commit recorded in `BUILD_INFO.txt`.

