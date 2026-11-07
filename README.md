# webm anything — Linux downloads

Prebuilt Linux binaries for **webm anything**, a desktop app and command-line
tool that turns a video file or link into a WebM, MP4 or GIF. WebM and MP4 are
squeezed to fit a size budget. GIFs use sensible defaults.

Each release has four files:

| File | What it is |
| --- | --- |
| `webm-anything-<version>-x86_64-unknown-linux-gnu.tar.gz` | The program, its license and third-party notices, a desktop entry and icon |
| `SHA256SUMS` | Checksums for the other three files |
| `install_release.py` | Installer, pinned to this release's version |
| `release_common.py` | Helper the installer needs; keep it next to the installer |

## Install

```sh
base=https://github.com/divine-divide/webm-anything-releases/releases/latest/download
curl -fsSLO "$base/SHA256SUMS"
curl -fsSLO "$base/install_release.py"
curl -fsSLO "$base/release_common.py"
sha256sum -c --ignore-missing SHA256SUMS
python3 install_release.py --prefix "$HOME/.local"
```

The installer downloads the archive from the same release, checks it against
`SHA256SUMS`, and only then extracts and runs it. It installs:

- `~/.local/bin/webm-anything`
- a menu entry and icon (skip them with `--binary-only`)
- the license and notices in `~/.local/share/doc/webm-anything/`

Run the same commands again to update. For a specific version, replace
`latest/download` with `download/v1.0.0` (or any tag on the
[releases page](https://github.com/divine-divide/webm-anything-releases/releases));
each release's installer is pinned to its own version.

Make sure `~/.local/bin` is on your `PATH`. To install an archive you already
downloaded, use
`python3 install_release.py --archive <file>.tar.gz --checksum SHA256SUMS`.

Don't pipe a downloaded script into a shell. A matching checksum shows the files
are the ones this release lists. It is not a publisher signature, and there is
no separate signing key.

## Requirements

| Needed | For | Install |
| --- | --- | --- |
| x86_64 Linux with glibc 2.38 or newer | everything | Ubuntu 24.04+, Debian 13+, Fedora 39+, Arch. musl (Alpine) and ARM are not supported. |
| **ffmpeg** and **ffprobe** | converting | `sudo apt install ffmpeg` · `sudo dnf install ffmpeg` · `sudo pacman -S ffmpeg` |
| Python 3.10 or newer | the installer only | Usually already installed |
| **yt-dlp** (optional) | links to pages such as YouTube or X | `pipx install yt-dlp`, or your package manager |
| Window libraries | the window only, not the command line | Usually already on a desktop system: EGL/GL, X11 or Wayland, xkbcommon |

Not bundled: ffmpeg, ffprobe and yt-dlp. Your ffmpeg needs `libvpx` (WebM),
`libx264` (MP4) and `libopus`/AAC for sound. On Fedora, use the RPM Fusion
`ffmpeg`, not `ffmpeg-free`. `webm-anything --version` lists what it found.

Window libraries by distribution, if the window does not open:

```sh
# Ubuntu / Debian
sudo apt install libegl1 libgl1 libx11-6 libx11-xcb1 libxcb1 libxi6 libxcursor1 \
    libxrender1 libwayland-client0 libwayland-egl1 libxkbcommon0 libxkbcommon-x11-0
# Fedora
sudo dnf install libglvnd-egl libglvnd-glx libX11 libX11-xcb libxcb libXi libXcursor \
    libXrender libwayland-client libwayland-egl libxkbcommon libxkbcommon-x11
```

## Quick check

```sh
webm-anything --version
webm-anything clip.mp4 -t 8 -o small.webm
webm-anything clip.mp4 -f mp4 -t 8
webm-anything clip.mp4 -f gif
webm-anything --help
```

## License

MIT. See `LICENSE`. Third-party notices for the bundled Rust crates and fonts
are inside each archive under `licenses/`.
