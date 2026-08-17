# ffmpeg-minimal-builds

Automated multi-platform builds of minimal, static `ffmpeg` and `ffprobe` binaries tailored specifically for H.264 & HEVC video transcoding with hardware acceleration and audio passthrough.

## Target Platform Matrix

| Platform | Arch | Hardware Acceleration | Software Codecs | Archive Name |
|:---|:---|:---|:---|:---|
| **Linux** | `x86_64` | NVENC (`cuda`), VA-API | `libx264`, `libx265` | `ffmpeg-minimal-linux-x86_64.tar.gz` |
| **Windows** | `x86_64` | NVENC, AMD AMF, Intel QSV (`libvpl`) | `libx264`, `libx265` | `ffmpeg-minimal-win-x86_64.zip` |
| **macOS** | `arm64` | Apple VideoToolbox | `libx264`, `libx265` | `ffmpeg-minimal-macos-arm64.tar.gz` |
| **macOS** | `x86_64` | Apple VideoToolbox | `libx264`, `libx265` | `ffmpeg-minimal-macos-x86_64.tar.gz` |

## Size Optimization

- Configured with `--disable-everything` and selective `--enable-*` flags.
- Symbol tables stripped (`strip`).
- Compressed with UPX (`upx --best`) on Linux and Windows.
- Total binary size: **~12–20 MB per archive** (down from ~160 MB+ full builds).

## Automation

- Checks for new stable FFmpeg release tags (`n*`) every Sunday at 12:00 PM IST (06:30 UTC).
- Automatically skips rebuilding if a GitHub Release for the current stable tag already exists.
- Publishes pre-built archives as GitHub Release assets tagged `ffmpeg-<tag>`.
