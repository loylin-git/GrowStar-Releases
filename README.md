# GrowStar · 成长星

**English** | [简体中文](README.zh-CN.md)

![Version](https://img.shields.io/badge/version-1.0.1-brightgreen)
![Platform](https://img.shields.io/badge/platform-Android%207.0%2B%20%7C%20Windows%2010%20(2004)%2B-blue)
![Network](https://img.shields.io/badge/network-none-success)
![Languages](https://img.shields.io/badge/i18n-%E7%AE%80%20%2F%20%E7%B9%81%20%2F%20EN-orange)
![License](https://img.shields.io/badge/license-proprietary-lightgrey)

<p align="center">
  <img src="assets/icon.png" width="120" alt="GrowStar icon" />
</p>

A private, offline-first growth tracker for children and teens. Log height and weight, watch them move along **CDC / WHO percentile curves**, and get an **adult height estimate** — all stored locally on your device, with **no network access, no account, and no tracking**.

---

## Download

| Platform | File | Notes |
| --- | --- | --- |
| Android | [**GrowStar-v1.0.1-android.apk**](https://github.com/loylin-git/GrowStar-Releases/releases/latest/download/GrowStar-v1.0.1-android.apk) · 19 MB | Android 7.0 (API 24) or later, ARM / ARM64. Sideloaded — see [Installing](#installing) |
| Windows | [**GrowStar-v1.0.1-windows-x64.zip**](https://github.com/loylin-git/GrowStar-Releases/releases/latest/download/GrowStar-v1.0.1-windows-x64.zip) · 37 MB | Windows 10 build 19041 or later, x64. Unzip and run `GrowthApp.exe` |

Browse all versions: **[Releases →](https://github.com/loylin-git/GrowStar-Releases/releases)**

---

## Features

### Recording
- **Multiple children** in one app, each with name, sex, birth date, parent heights, and a photo avatar (pan & zoom to frame, auto-downscaled to 512 px).
- **Height / weight entries** with second-precision timestamps; one record per day, merged automatically.
- **BMI** computed on the fly; desktop shows a sortable table, phones show cards.

### Charts
- **Height, weight, and BMI curves** drawn natively (no third-party chart library), with zoom, pan, and pinch-to-zoom.
- **Reference percentile bands** P3 / P15 / P50 / P85 / P97 from **CDC 2000** or **WHO 2007**, switchable inside the chart.
- A **predicted growth path** (dashed) continuing from the latest measurement to the estimated adult height, plus a vertical marker at today's date.
- **Share the chart** as an image through the system share sheet.

### Prediction
- Under 4 years old, or with no measurement yet: **mid-parental (Tanner) method**.
- From 4 years old with a measurement: **Khamis-Roche** blended with the child's **current percentile position**, weighted by age — so the estimate starts anchored to the real measurement and converges to a stable adult value.
- Adult endpoint fixed at **19 years**; the curve never predicts a decrease in height.

### Data & settings
- **Import / export** in **JSON (default)**, CSV, XLSX, or SQLite — covering users (including avatars as embedded base64), growth records, and app settings. CSV is auto-detected as UTF-8, UTF-8 BOM, or GBK/GB18030, so Excel files open without mojibake.
- **Units**: cm ↔ inch, kg ↔ lb. Data is always stored metric; conversion happens at display and entry only.
- **Interface language**: Simplified Chinese, Traditional Chinese, English — switches instantly, no restart.
- **Theme color** per child; page background, cards, and text are derived from the chosen accent.
- **Disclaimer** and **privacy policy** built in; **share the app** with friends via the system share sheet; **exit confirmation** so a stray back-press cannot discard work.

---

## Privacy

- The app **contains no networking code** — no HTTP client, no ads, no analytics, no crash reporting. The `INTERNET` permission is not even declared.
- All data lives in a local SQLite database and an on-device avatars folder.
- The only permission requested is file read access for import/export and avatar picking (Android ≤ 12).
- Deleting a child's profile, clearing app data, or uninstalling **permanently removes** the data — use Export regularly to keep your own backup.

---

## How the prediction works

Reference data is stored as **LMS parameters** (L = skewness, M = median, S = coefficient of variation) from:

- **CDC Growth Charts (2000)** — length/height-for-age, weight-for-age, BMI-for-age
- **WHO Child Growth Standards / Growth Reference (2007)**

Percentiles are computed from LMS with a probit (inverse normal) transform. For adult height:

1. `P` — percentile of the Khamis-Roche adult estimate on the 19-year curve.
2. `Q` — percentile of the latest measured height at the child's current age.
3. `w = (age − 4) / 15`, clamped to 0–1: `R = w·P + (1 − w)·Q`.
4. Adult height = the height at percentile `R` on the 19-year curve.

The future curve follows the `R` percentile line with a time remap, so it starts exactly at the last measurement and ends exactly at the predicted adult height.

---

## Installing

**Android** — the APK is not distributed through Google Play, so allow installation from your browser/files app when prompted ("Install unknown apps"), then open the APK. Android may show a Play Protect notice for sideloaded apps; choose **Install anyway**.

**Windows** — unzip to any folder and run `GrowthApp.exe`. The build is self-contained (includes the Windows App SDK runtime), so nothing else needs installing. Because it is not signed with a commercial certificate, SmartScreen may say "Windows protected your PC" — click **More info → Run anyway**. Your data is stored under `%LOCALAPPDATA%`, not next to the exe, so it survives moving the folder.

---

## FAQ

**Is the predicted height reliable?**
It is a statistical estimate with an error range of several centimetres, not a clinical assessment. It updates as you add measurements. See the in-app Disclaimer for full limitations.

**Can I move data between phones?**
Yes: Export → JSON on the old device, transfer the file, Import → JSON on the new one. JSON keeps users, avatars, records, and settings in a single file.

**CDC or WHO — which should I use?**
WHO 2007 describes how children *should* grow (standards, multi-country sample); CDC 2000 describes how a US sample *did* grow (reference). For rough tracking either works; stay consistent with the one you started with.

**Does it sync to the cloud?**
No. There is no server and no sync. Export files are the only way data leaves the device, and you choose where they go.

---

## Disclaimer

This app is a family record-keeping tool, **not a medical device**. Height, weight, BMI, percentiles, growth curves, and adult height predictions are estimates derived from public population statistics. They are not intended for screening, diagnosis, treatment, or medication decisions. Always consult a qualified pediatrician or endocrinologist for concerns about growth, nutrition, puberty, or bone age.

CDC and WHO names and data belong to their respective organizations. This is an independent third-party work and is **not authorized, endorsed, or sponsored by CDC or WHO**.

---

## License

**Proprietary.** The source code is not open; this repository hosts documentation and release artifacts only. You may freely download, install, and use the released binaries, and share the download link. Redistribution of modified builds is not permitted.

Reference growth data is used read-only for local computation (see Disclaimer above).

---

## Links

- **Releases & downloads:** <https://github.com/loylin-git/GrowStar-Releases/releases>
- **Issues & feedback:** <https://github.com/loylin-git/GrowStar-Releases/issues>
