# Loading Frames — Complete Feature Reference

**Version 1.0 · macOS 13+ · Apple Silicon**
*Made by DarkModeEverything*

---

## Overview

Loading Frames is a native macOS video downloader built entirely with SwiftUI. It provides a clean, dark-mode interface for downloading video and audio content from over 1000 websites, with no command-line knowledge required. All processing happens locally on your Mac — no cloud, no account, no tracking.

---

## Supported Platforms

Loading Frames automatically detects the source platform from any pasted URL. Supported platforms include:

- **YouTube** — videos, Shorts, Music topics, age-restricted content (with authentication), playlists
- **Instagram** — Reels, posts, stories
- **TikTok** — videos without watermarks
- **Twitter / X** — video tweets
- **Vimeo** — public and unlisted videos
- **SoundCloud** — tracks and playlists
- **Facebook** — public video posts
- **Reddit** — video posts
- **Twitch** — clips and VODs
- **Dailymotion** — videos
- **1000+ other sites** — powered by yt-dlp's universal extractor engine

No configuration is needed to switch between platforms — paste any link and the app handles the rest automatically.

---

## Download Quality

### Video Quality Modes

Loading Frames offers two quality modes, selectable before each download:

#### Normal — MP4 (QuickTime Compatible)

Downloads the best available streams, then encodes to H.264 MP4 using Apple's VideoToolbox hardware encoder. The resulting file is universally compatible with QuickTime Player, Preview, iMovie, Final Cut Pro, and all Apple devices. Encoding is performed on the dedicated media engine of Apple Silicon chips, keeping CPU usage and fan noise minimal.

Target bitrates by resolution:

| Resolution | Target Bitrate |
|---|---|
| 4K (2160p) | 20 Mbps |
| 1440p | 12 Mbps |
| 1080p | 8 Mbps |
| 720p and below | 4 Mbps |

#### Best — MKV (Original Quality)

Downloads the original video and audio streams as-is, merged into an MKV container with zero re-encoding. This preserves the exact quality served by the source platform — no generation loss, no compression artifacts, no quality decisions made by the app. Files are significantly smaller than re-encoded versions at equivalent visual quality. Recommended for archiving and professional use.

> MKV files play natively in VLC. They can also be converted to MP4 with a single click inside the app.

### Available Resolutions

- 4K (2160p)
- 1440p
- 1080p
- 720p
- Audio MP3 only

The app detects the maximum available resolution before downloading and automatically adjusts if the requested resolution exceeds what the source provides, showing an informational note on the download item.

### Audio-Only Mode

Selecting Audio MP3 downloads only the audio stream, extracted and converted to MP3 format.

| Mode | Bitrate |
|---|---|
| Normal | 192 kbps |
| Best | 320 kbps |

---

## Download Management

### Download Now
Immediately starts fetching metadata and downloading the pasted URL. The item appears in the queue instantly and progresses through: fetching info → downloading → encoding → completed.

### Add to Queue
Adds the URL to the queue without starting the download. Metadata and thumbnails are fetched automatically in the background so you can see the video title, duration, and thumbnail before deciding to download. Useful for preparing a batch of downloads to run together.

### Queue System
Multiple downloads can run simultaneously. Each item in the queue shows:

- Video thumbnail (fetched before download)
- Title
- Platform and duration
- Selected resolution and quality mode
- Real-time download progress bar (green, 0–100%)
- Encoding progress bar (blue, 0–100% with time remaining)
- Conversion progress bar (purple, 0–100% with time remaining)

### Batch Downloads
Select multiple queued items using checkboxes and download them all simultaneously with the **Download Selected** button. The **Download All** button starts all idle items at once.

### Playlist Support
Paste a YouTube playlist URL and the app detects it automatically, showing a playlist preview card with the title, thumbnail, and number of videos. Pressing **Download Now** starts all videos immediately. Pressing **Add to Queue** adds every video individually to the queue, where they can be managed separately.

---

## Pre-Fetch System

When a URL is pasted into the search field, Loading Frames immediately begins fetching metadata in the background — before the download button is pressed. Within seconds, a preview card appears showing:

- Video thumbnail
- Full title
- Platform name
- Duration
- Maximum available resolution

This allows you to confirm you have the right video before committing to a download. The fetched data is reused when the download starts, so no time is wasted re-fetching information.

For playlist URLs, the preview card shows the playlist title, thumbnail, and total number of videos.

---

## File Management

### Output Location
Downloaded files are saved to a dedicated **"Loading Frames Downloads"** folder, created automatically inside your chosen download directory. The default location is `~/Downloads/Loading Frames Downloads/`. The folder is created silently on first download if it does not exist.

The output directory can be changed at any time in Settings.

### File Naming
Files are named using the following format:

```
Video Title - YYYYMMDD-HHmm - Resolution.extension
```

Example: `Interstellar Official Trailer - 20240702-1430 - 1080p.mp4`

Audio files follow the same pattern with "audio" in place of resolution.

### Per-Item Actions
Each completed download item shows action buttons:

| Button | Action |
|---|---|
| 📁 Open file location | Reveals the file in Finder |
| 🔄 Convert to MP4 | Converts MKV to QuickTime-compatible MP4 (Best quality only) |
| 🖼 Download thumbnail | Saves the video thumbnail as a separate JPEG file |
| 🔁 Download again | Re-queues the item without pasting the URL again |
| ✕ Remove | Removes the item from the list |

Failed items additionally show:

| Button | Action |
|---|---|
| ↺ Retry | Retries the download |
| 📋 Copy error log | Copies the full error message to the clipboard |

---

## MKV to MP4 Conversion

Best quality downloads produce MKV files. For users who need QuickTime compatibility, a one-click conversion is available directly inside the app.

The conversion process:
1. Probes the source file to determine resolution
2. Encodes video to H.264 using Apple VideoToolbox hardware encoder
3. Encodes audio to AAC at 256 kbps
4. Outputs a FastStart MP4 (optimised for playback and sharing)
5. Deletes the original MKV after successful conversion

A real-time purple progress bar shows percentage and estimated time remaining during conversion.

---

## Views

### List View
The default view. Each download item appears as a horizontal row with thumbnail, title, metadata, status, and action buttons. Optimised for scanning many items quickly.

### Grid View
A visual card-based layout with 16:9 thumbnails, a progress line at the bottom of each card, and a status badge overlaid on the thumbnail. Optimised for browsing content visually.

The selected view mode is remembered across sessions.

---

## YouTube Authentication

Some YouTube content requires login to download — including age-restricted videos, members-only content, and certain region-locked videos. Loading Frames handles this through a one-time browser cookie setup.

### First-Launch Setup
On first launch, a setup assistant guides you through authentication:

1. Select your browser
2. The app reads your existing YouTube login cookies from the browser
3. Cookies are saved locally to `~/.config/loading-frames/cookies.txt`
4. Authentication is complete — no passwords are stored or transmitted

### Refreshing Authentication
If cookies expire, authentication can be refreshed from **Settings → YouTube Authentication → Refresh**.

### Supported Browsers

| Browser | |
|---|---|
| Safari | ✅ |
| Google Chrome | ✅ |
| Mozilla Firefox | ✅ |
| Brave Browser | ✅ |
| Opera | ✅ |
| Microsoft Edge | ✅ |
| Vivaldi | ✅ |
| Arc | ✅ |

Only browsers installed on your Mac are shown in the setup screen.

---

## Settings

| Setting | Description |
|---|---|
| Download Location | Choose any folder as the base download directory |
| Default Resolution | Pre-selected resolution when the app opens |
| Default Bitrate | Pre-selected quality mode (Normal or Best) at launch |
| YouTube Authentication | View cookie status and refresh authentication |

All settings are saved automatically and persist across sessions.

---

## Notifications

When a download completes, Loading Frames sends a macOS system notification showing the file title and destination folder. A toast confirmation also appears at the bottom of the app window with an **Open Folder** button.

---

## Technical Foundation

| Component | Technology |
|---|---|
| UI framework | SwiftUI (native macOS) |
| Download engine | yt-dlp (bundled, no Homebrew required) |
| Media processing | ffmpeg + ffprobe (bundled) |
| Video encoding | Apple VideoToolbox (hardware H.264) |
| Concurrency | Swift async/await |
| Storage | UserDefaults |
| Notifications | UNUserNotificationCenter |

> yt-dlp, ffmpeg, and ffprobe are bundled inside the app. No external dependencies, Homebrew installation, or Terminal commands are required by the end user.

---

## System Requirements

| Requirement | Minimum |
|---|---|
| macOS | 13 Ventura or later |
| Architecture | Apple Silicon (M1 or later) |
| Disk space | ~220 MB for the app |
| Internet | Required for downloading |

> Optimised for macOS 26 Tahoe with native Liquid Glass UI elements.

---

## Privacy

- No analytics, telemetry, or usage tracking of any kind
- No account required
- All downloads happen locally on your device
- Cookie files are stored locally and never transmitted
- The app does not communicate with any server other than the video source

---

## Support the Project

If you find Loading Frames useful, consider supporting its development:

- ☕ [Ko-fi](https://ko-fi.com/darkmodeeverything)
- ⭐ Star the repo on [GitHub](https://github.com/darkmode-everything/loading-frames)

---

*Loading Frames is developed and maintained by DarkModeEverything.*
*Powered by the open-source projects [yt-dlp](https://github.com/yt-dlp/yt-dlp) and [ffmpeg](https://ffmpeg.org).*
