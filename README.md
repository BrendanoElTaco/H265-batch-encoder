# H265 Video Re-encoding Script (with HEVC/AV1 Detection, Recompression, Adaptive CQ & Auto-Mounting)

## 📦 Description

This Bash script scans a specified folder (optionally recursively) for video files (`*.mkv`, `*.avi`, `*.mp4`, `*.mov`, `*.wmv`, `*.flv`) that are **not already encoded in HEVC (H.265) or AV1**, and re-encodes them using **GPU-accelerated ffmpeg** to reduce size. It performs **sample-based test encodes** to determine if recompression will save space, and optionally replaces the originals (or saves backups).

It includes **automatic CQ (Constant Quality) adjustment based on resolution**, detailed logging, error handling, and **optional auto-mounting of network shares**.

---

## 🎯 Features

- ✅ Automatically mounts network shares on launch (configurable paths)
- ✅ Skips files already encoded in **HEVC** or **AV1**
- ✅ Adaptive **Constant Quality (CQ)** based on resolution
- ✅ **Sample-based analysis**: three test encodes (5s) to estimate final size
- ✅ Skips encoding if expected size reduction < 20%
- ✅ Skips files smaller than a specified size threshold
- ✅ Skips very low bitrate files automatically
- ✅ Keeps all **audio/subtitle tracks**, unless errors are encountered
- ✅ Replaces original only if new file is smaller and durations match
- ✅ Optionally backs up originals (`-backup /path`)
- ✅ Supports stopping after a set time (`--stop-after HH.5`)
- ✅ Logs results in `encoded.list` and `failed.list`
- ✅ Prints final size savings in GB and percentage
- ✅ Supports **dry-run**, **clean**, and **purge** operations

---

## ⚙️ Requirements

- `ffmpeg` with NVENC (for GPU encoding)
- `ffprobe` (comes with ffmpeg)
- GNU tools (`find`, `stat`, `timeout`, `awk`, etc.)
- NVIDIA GPU with CUDA support (optional, but recommended)

---

## 🧪 How it works

1. **Mounts network shares** to `/mnt/wshare`, `/mnt/xshare`, etc. (customizable)
2. For each video file:
   - Checks if codec is already HEVC or AV1
   - Checks file size, duration, and bitrate
   - Performs 3 short test encodes to predict savings
   - Skips if prediction shows < 20% space saved
   - Encodes with appropriate CQ based on resolution (SD vs HD)
   - Replaces or saves as new if smaller and valid
   - Logs results in `encoded.list` or `failed.list`
3. At end, prints total files processed and GB/percent saved

---

## 🧾 Usage

```bash
./script.sh [options] <folder>

Options:
  -R                  Recursively scan subfolders
  min=X.YZ            Ignore files smaller than X.YZ GB
  test=N              Use N seconds for test encodes (default: 5)
  --dry-run           Show files eligible for encoding, no changes
  --keep-original     Save output as new file, don’t overwrite
  --allow-h265        Re-encode even if already HEVC
  --allow-av1         Re-encode even if already AV1
  -backup /path       Path to backup original files before replacement
  --clean             Delete temporary test/encode files
  --purge             Delete encoded.list and failed.list logs
  --stop-after H      Stop after H hours (e.g., 4, 2.5)
  -h                  Show help message and exit
```

---

## 🧮 Encoding Summary Example

```bash
📦 Encoding Summary
──────────────────────
🔹 Total files processed: 48
🔹 Original size: 105.73 GB
🔹 New size:      62.48 GB
🔹 Space saved:   43.25 GB (40%)
```
