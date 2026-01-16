# 💧 Water Meter Photo Logger

A mobile-first web application for capturing and logging water meter readings with automatic metadata extraction, GPS-based timezone detection, and cloud storage integration.

![Water Meter Logger](https://img.shields.io/badge/version-1.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## 🌟 Features

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
  - **EXIF datetime**: Uses photo's original timestamp
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

---

## 🚀 Quick Start

### Deployment (GitHub Pages)

1. **Fork or clone this repository**
   ```bash
   git clone https://github.com/jahrling/water-meter-logger.git
   ```

2. **Enable GitHub Pages**
   - Go to repository Settings
   - Navigate to Pages section
   - Select `main` branch and root folder
   - Save

3. **Access your app**
   - URL: `https://jahrling.github.io/water-meter-logger/exif-extractor.html`
   - Bookmark it or add to home screen on mobile

### Basic Usage

1. Open the app in Safari (iOS) or Chrome (Android)
2. Tap "Choose File" or drag-and-drop a photo
3. Review extracted GPS and timestamp data
4. Enter meter details (Org ID, Device ID, meter name, reading)
5. Select timezone (auto-detected if GPS available)
6. Tap "Save Image & JSON" to download files

---

## ⚙️ Configuration

### GPS Timezone Detection (Included ✅)

The Geoapify API key is **pre-configured** with 3,000 free requests per day.

**How it works:**
- Photo with GPS → Automatic timezone detection
- Green notification appears: "📍 Timezone Detected from GPS"
- Dropdown auto-updates to correct timezone

**Supported regions:** USA, Canada, Europe, Asia, Australia, South America, Africa (50+ timezones mapped)

### SharePoint Integration (Optional)

To enable the "Save to SharePoint" button:

1. **Register Azure AD Application**
   - Go to [Azure Portal](https://portal.azure.com)
   - Navigate to: Azure Active Directory → App registrations → New registration
   - Fill in:
     ```
     Name: Water Meter Photo Logger
     Supported account types: Accounts in this organizational directory only
     Redirect URI: Single-page application (SPA)
     URL: https://jahrling.github.io/water-meter-logger/
     ```

2. **Configure API Permissions**
   - Add Microsoft Graph permissions:
     - `Files.ReadWrite.All`
     - `Sites.ReadWrite.All`
   - Grant admin consent

3. **Add Client ID to Code**
   - Copy Application (client) ID from Azure
   - Edit `exif-extractor.html`, find line ~740:
     ```javascript
     clientId: "YOUR_AZURE_APP_CLIENT_ID",
     ```
   - Replace with your actual client ID
   - Commit and push changes

4. **Update Target Folder** *(optional)*
   - Edit `sharePointConfig` object (line ~750):
     ```javascript
     const sharePointConfig = {
         siteUrl: "your-tenant.sharepoint.com",
         sitePath: "/sites/YourSite",
         libraryName: "Shared Documents",
         folderPath: "Your/Folder/Path"
     };
     ```

📖 See [SHAREPOINT_SETUP.md](SHAREPOINT_SETUP.md) for detailed instructions.

---

## 📱 Mobile Installation

### iOS (Safari)

1. Open the app in Safari
2. Tap the Share button
3. Select "Add to Home Screen"
4. Tap "Add"

The app will now appear on your home screen like a native app!

### Android (Chrome)

1. Open the app in Chrome
2. Tap the three-dot menu
3. Select "Add to Home screen"
4. Tap "Add"

---

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

### Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `filename` | string | Original photo filename |
| `orgId` | string | Organization identifier |
| `deviceId` | string | Device/sensor identifier |
| `meterName` | string | Human-readable meter name |
| `meterType` | string | `"single"` or `"compound"` |
| `meterReadings.reading1` | number | Primary reading (always required) |
| `meterReadings.reading2` | number | Secondary reading (compound meters only) |
| `meterReadings.units` | string | `"gallons"`, `"liters"`, `"cubic_feet"`, or `"cubic_meters"` |
| `OffsetTimeOriginal` | string | Timezone of photo (e.g., `"UTC-06:00"`) |
| `timestamp_utc` | string | Timestamp in UTC (ISO 8601) |
| `DateTimeOriginal` | string | Original timestamp from EXIF (ISO 8601) |
| `gps.latitude` | number | Decimal degrees latitude |
| `gps.latitudeRef` | string | `"N"` or `"S"` |
| `gps.longitude` | number | Decimal degrees longitude |
| `gps.longitudeRef` | string | `"E"` or `"W"` |
| `gps.gpsVersionID` | string | GPS version from EXIF |

---

## 🔧 Technical Details

### Architecture
- **Pure HTML/CSS/JavaScript** - No build process required
- **Single file application** - Easy to deploy and maintain
- **Progressive Web App** (PWA-ready) - Can be added to home screen

### Dependencies (CDN)
- [exif-js](https://github.com/exif-js/exif-js) - EXIF metadata extraction
- [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js) - Microsoft authentication (for SharePoint)

### APIs Used
- **Geoapify Reverse Geocoding API** - Timezone detection from coordinates
- **Microsoft Graph API** - SharePoint file upload (optional)

### Browser Compatibility
- ✅ iOS Safari 12+
- ✅ Android Chrome 80+
- ✅ Desktop Chrome, Edge, Firefox, Safari

### Privacy & Security
- **No data collection** - All processing happens in browser
- **No server backend** - Direct client-to-API communication
- **OAuth 2.0** - Secure authentication for SharePoint
- **API key included** - Pre-configured Geoapify key (public tier)

---

## 🐛 Troubleshooting

### GPS/Timezone Issues

**Problem:** GPS location detected but timezone doesn't update
- **Solution:** Check the debug panel (black console at bottom). If timezone name appears but isn't mapped, open an issue with the timezone name.

**Problem:** No GPS data extracted
- **Cause:** Browser camera photos don't include GPS metadata
- **Solution:** Use photos from your native Camera app with location services enabled

### File Saving Issues

**Problem:** iOS saves to Downloads instead of prompting for location
- **Solution:** This is expected behavior. Use the Files app to move files to OneDrive or iCloud Drive after download.

**Problem:** "SharePoint not configured" error
- **Solution:** Follow [SHAREPOINT_SETUP.md](SHAREPOINT_SETUP.md) to register an Azure AD app and add the client ID.

### Photo Upload Issues

**Problem:** Upload button doesn't work on iOS
- **Solution:** Ensure you're using Safari. Other browsers may have restrictions.

**Problem:** Photo appears upside down or rotated
- **Solution:** Use the rotation buttons (⟲ ⟳) at the bottom-left of the image.

---

## 📋 Known Limitations

- **Browser camera photos** have no EXIF data (use native Camera app instead)
- **Timezone mapping** requires pre-defined list (currently 50+ timezones)
- **Large files (>10MB)** may be slow to upload to SharePoint
- **Offline mode** not supported (requires internet for timezone lookup)
- **Desktop use** works but is optimized for mobile

---

## 🛣️ Roadmap

- [ ] Add offline mode with cached timezone database
- [ ] Support for video uploads
- [ ] Batch upload multiple photos
- [ ] Historical reading charts
- [ ] Export to CSV
- [ ] Dark mode
- [ ] Multi-language support
- [ ] Progressive Web App (PWA) with service worker

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Development Setup

1. Clone the repository
2. Open `exif-extractor.html` in your browser
3. Make changes
4. Test on both desktop and mobile
5. Submit PR

### Adding New Timezones

To add a timezone mapping:

1. Find the `getUTCOffsetFromTimezone()` function
2. Add your timezone to the `timezoneMap` object:
   ```javascript
   'America/Argentina/Buenos_Aires': 'UTC-03:00',
   ```
3. Test with a photo from that location
4. Submit PR

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details

---

## 🙏 Acknowledgments

- **EXIF.js** - EXIF metadata extraction library
- **Geoapify** - Free geocoding and timezone API
- **Microsoft** - Azure AD and Graph API documentation
- **Anthropic Claude** - Development assistance

---

## 📧 Support

Having issues? Please open an [issue on GitHub](https://github.com/jahrling/water-meter-logger/issues).

---

## 📸 Screenshots

### Mobile View
*Upload a photo and see GPS data automatically extracted*

### Timezone Detection
*Green notification appears when timezone is auto-detected*

### Save Options
*Choose to save locally or directly to SharePoint*

---

**Made with ❤️ for water meter monitoring**
**Made with Claude.ai**
