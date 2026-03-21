# 💧 Water Meter Photo Logger

A mobile-first web toolkit for capturing and logging water meter readings — with automatic EXIF metadata extraction, GPS-based timezone detection, OCR digit recognition, and cloud storage integration.

![Water Meter Logger](https://img.shields.io/badge/version-2.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Platform](https://img.shields.io/badge/platform-mobile%20%7C%20desktop-brightgreen.svg)

-----

## 🌟 Features

### 🔢 Meter OCR — Swipe to Read *(New in v2.0)*

An in-browser OCR tool that lets you swipe across meter numbers to extract digits automatically.

- **Swipe-to-select** — draw your finger across the meter display; a translucent band tracks your path
- **Smart cropping** — on release, the region is cropped from the full-resolution original image (not the downscaled display), upscaled 2×, and preprocessed with grayscale conversion, contrast stretching, and binarization
- **Tesseract.js OCR** — runs entirely in the browser with no server, no API key, and no cost; character whitelist locked to `0123456789.`, page segmentation set to single text line mode
- **Editable results** — OCR output appears in an editable field with a confidence percentage so the user can verify or correct before submitting
- **Visual feedback** — green translucent band while swiping, dashed bounding box with corner markers on release, crop preview in a slide-up panel

### 📸 Photo Capture & Upload

- **Mobile-optimized** interface works on iOS and Android
- Upload photos from camera or photo library
- Support for JPEG, PNG formats
- Automatic EXIF metadata extraction
- Touch-friendly image manipulation (pinch zoom, pan, rotate)

### 🗺️ GPS & Timezone Detection

- **Automatic GPS coordinate extraction** from photo metadata
- **Smart timezone detection** using Geoapify API
- Displays latitude, longitude, and hemispheric references
- Real-time timezone recommendation based on location
- Visual confirmation with green notification

### ⏰ Intelligent Timestamp Handling

- **Three automatic modes**:
  - **EXIF datetime**: Uses photo’s original timestamp
  - **Capture time**: For browser camera photos (< 10 seconds old)
  - **Manual entry**: Prompts user for old photos without metadata
- Timezone-aware UTC conversion
- Dual display: Original time (local) and UTC time

### 📊 Meter Reading Management

- **Single meter** or **compound meter** support
- Flexible units: Gallons, Liters, Cubic Feet, Cubic Meters
- Organization ID, Device ID, and Meter Name fields
- Structured JSON output with all metadata

### 💾 Multiple Save Options

- **Local/OneDrive Save**: Downloads files to iOS Files app
  - User selects destination (OneDrive, iCloud Drive, etc.)
  - Saves both image and JSON metadata
  - Two-step process for iOS compatibility
- **SharePoint Integration** *(optional)*:
  - Direct upload to configured SharePoint folder
  - Azure AD authentication with SSO support
  - Batch upload (image + JSON)
  - Pre-configured for: `/sites/MarlinProductEngineering2/Shared Documents/Mobile/EXIF`

### 🐛 Developer-Friendly

- **iOS Debug Console**: Real-time debugging visible on-screen
- Comprehensive console logging
- Error handling with user-friendly messages
- No external dependencies except CDN libraries

-----

## 📁 Repository Contents

|File                     |Description                                                                          |
|-------------------------|-------------------------------------------------------------------------------------|
|`index.html`             |**Meter OCR tool** — swipe-to-select + Tesseract.js digit recognition                |
|`exif-extractor.html`    |**EXIF extraction tool** — photo metadata, GPS, timezone, meter readings, JSON export|
|`README.md`              |This file                                                                            |
|`LICENSE`                |MIT License                                                                          |
|`CHANGELOG.md`           |Version history                                                                      |
|`CONTRIBUTING.md`        |Contributor guidelines                                                               |
|`SHAREPOINT_SETUP.md`    |Azure AD app registration for SharePoint uploads                                     |
|`TIMEZONE_DEBUG_GUIDE.md`|Debugging GPS timezone detection                                                     |

-----

## 🚀 Quick Start

### Deployment (GitHub Pages)

1. **Fork or clone this repository**
   
   ```bash
   git clone https://github.com/jahrling/EXIFtest.git
   ```
1. **Enable GitHub Pages**
- Go to repository Settings → Pages
- Select `main` branch and root folder
- Save
1. **Access your tools**
- OCR Tool: `https://jahrling.github.io/EXIFtest/`
- EXIF Tool: `https://jahrling.github.io/EXIFtest/exif-extractor.html`
- Bookmark or add to home screen on mobile

-----

## 🔢 Using the OCR Tool

1. Open `index.html` on your phone
1. Tap the camera icon to upload or take a meter photo
1. **Swipe your finger** across the meter numbers — a green band follows your path
1. When you lift your finger, a dashed bounding box and crop preview appear
1. Tap **Read Numbers** to run OCR
1. Review the extracted digits (editable) and confidence score
1. Tap **Redo** to re-swipe, or **↩** to undo

### OCR Preprocessing Pipeline

The tool applies the following transforms before OCR to maximize accuracy:

```
Original crop → 2× upscale → Grayscale → Contrast stretch → Binary threshold → Tesseract
```

Tesseract configuration:

- Language: `eng`
- Character whitelist: `0123456789.`
- Page segmentation mode: `7` (single text line)

### Tips for Best OCR Results

- **Get close** — fill the frame with the meter display
- **Good lighting** — avoid shadows across the digits
- **Swipe tightly** — follow the number row closely; a smaller region means less noise
- **Straight angle** — shoot the meter face-on when possible

-----

## 📸 Using the EXIF Extraction Tool

1. Open `exif-extractor.html` on your phone
1. Tap to upload or take a meter photo
1. Review extracted GPS and timestamp data
1. Enter meter details (Org ID, Device ID, meter name, reading)
1. Select timezone (auto-detected if GPS available)
1. Tap **Save Image & JSON** to download files

-----

## ⚙️ Configuration

### GPS Timezone Detection (Included ✅)

The Geoapify API key is **pre-configured** with 3,000 free requests per day.

**How it works:**

- Photo with GPS → Automatic timezone detection
- Green notification appears: “📍 Timezone Detected from GPS”
- Dropdown auto-updates to correct timezone

### SharePoint Integration (Optional)

To enable SharePoint uploads:

1. **Register Azure AD Application** at [portal.azure.com](https://portal.azure.com)
1. **Configure API Permissions**: `Files.ReadWrite.All`, `Sites.ReadWrite.All`
1. **Add Client ID** to `exif-extractor.html` (line ~740)
1. **Update Target Folder** in `sharePointConfig` object

📖 See <SHAREPOINT_SETUP.md> for detailed instructions.

-----

## 📱 Mobile Installation

### iOS (Safari)

1. Open the app in Safari
1. Tap the Share button
1. Select “Add to Home Screen”
1. Tap “Add”

### Android (Chrome)

1. Open the app in Chrome
1. Tap the three-dot menu
1. Select “Add to Home screen”
1. Tap “Add”

The app will appear on your home screen like a native app.

-----

## 🗂️ JSON Output Format

```json
{
  "filename": "IMG_1234.jpg",
  "orgId": "ORG123",
  "deviceId": "DEV456",
  "meterName": "Building A Main",
  "meterType": "single",
  "meterReadings": {
    "reading1": 12345.67,
    "units": "gallons",
    "reading2": 67890.12
  },
  "OffsetTimeOriginal": "UTC-06:00",
  "timestamp_utc": "2024-01-15T20:30:45.000Z",
  "DateTimeOriginal": "2024-01-15T14:30:45.000Z",
  "gps": {
    "latitude": 41.878100,
    "latitudeRef": "N",
    "longitude": -87.629800,
    "longitudeRef": "W",
    "gpsVersionID": "2.2.0.0"
  }
}
```

-----

## 🔧 Technical Details

### Architecture

- **Pure HTML/CSS/JavaScript** — no build process required
- **Single-file applications** — easy to deploy and maintain
- **Progressive Web App** (PWA-ready) — can be added to home screen
- **100% client-side** — no data leaves the browser (except optional SharePoint upload and Geoapify timezone lookup)

### Dependencies (CDN)

|Library                                                                      |Used In  |Purpose                            |
|-----------------------------------------------------------------------------|---------|-----------------------------------|
|[exif-js](https://github.com/exif-js/exif-js)                                |EXIF tool|EXIF metadata extraction           |
|[Tesseract.js v5](https://github.com/naptha/tesseract.js)                    |OCR tool |In-browser OCR engine              |
|[MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|EXIF tool|Microsoft authentication (optional)|

### APIs Used

- **Geoapify Reverse Geocoding API** — timezone detection from GPS coordinates
- **Microsoft Graph API** — SharePoint file upload (optional)

### Browser Compatibility

- ✅ iOS Safari 14+
- ✅ Android Chrome 80+
- ✅ Desktop Chrome, Edge, Firefox, Safari

### Privacy & Security

- **No data collection** — all processing happens in browser
- **No server backend** — direct client-to-API communication
- **OCR runs locally** — Tesseract.js processes images entirely on-device
- **OAuth 2.0** — secure authentication for SharePoint

-----

## 📋 Changelog

### v2.0.0 — Meter OCR

**Added:**

- `index.html` — Swipe-to-select OCR tool for reading meter digits
- Tesseract.js integration (fully in-browser, no API key, no cost)
- Touch-based swipe path drawing with visual feedback
- Full-resolution crop from original image with 2× upscale
- Image preprocessing pipeline (grayscale, contrast stretch, binarization)
- Editable OCR result field with confidence percentage
- Dark mobile-first UI with JetBrains Mono + Outfit typography

### v1.0.0 — EXIF Extraction

- `exif-extractor.html` — Photo metadata extraction and meter reading logger
- GPS coordinate extraction and timezone detection
- Single and compound meter support
- JSON export with structured metadata
- SharePoint integration (optional)
- Image zoom, pan, and rotate

-----

## 🐛 Troubleshooting

### OCR Tool

**Problem:** OCR returns garbled text or low confidence

- Ensure good lighting and a straight-on angle
- Swipe tightly along the digit row — avoid including extra text or graphics
- Try the “Redo” button and swipe more precisely

**Problem:** Tesseract takes a long time to load

- First load downloads the OCR engine (~2-4 MB over CDN); subsequent runs in the same session are instant

**Problem:** Swipe doesn’t register

- Make sure you’re swiping on the image area, not the header or panel

### EXIF Tool

**Problem:** GPS location detected but timezone doesn’t update

- Check the debug panel (black console at bottom)

**Problem:** “Save to SharePoint” button doesn’t appear

- Azure AD Client ID must be configured (see <SHAREPOINT_SETUP.md>)

**Problem:** Files won’t open on iOS

- HTML files must be accessed via a URL (GitHub Pages), not from the Files app

-----

## 🛣️ Roadmap

- [ ] Integrate OCR results into the EXIF/JSON export flow
- [ ] Add serial number / meter ID recognition
- [ ] Offline support via service worker
- [ ] Batch processing for multiple photos
- [ ] Export to CSV / Excel
- [ ] Power Automate integration for SharePoint workflows

-----

## 📄 License

MIT License — see <LICENSE> for details.