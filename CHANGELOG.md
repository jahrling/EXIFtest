# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-01-16

### Added
- Initial release
- Photo upload from camera or file library
- EXIF metadata extraction
- GPS coordinate extraction with display panel
- Automatic timezone detection using Geoapify API
- Smart timestamp handling (EXIF, capture time, or manual entry)
- Image manipulation (pinch zoom, pan, rotate)
- Single and compound meter reading support
- Multiple unit types (gallons, liters, cubic feet, cubic meters)
- Dual save options (local/OneDrive and SharePoint)
- iOS Files app integration for OneDrive saving
- Azure AD authentication for SharePoint upload
- On-screen debug console for mobile debugging
- Mobile-optimized responsive design
- Touch-friendly controls and gestures
- Structured JSON output with all metadata
- Pre-configured Geoapify API key (3,000 requests/day)
- 50+ timezone mappings for global coverage
- Comprehensive documentation and setup guides

### Features Highlights

#### GPS & Location
- Automatic GPS extraction from photo EXIF
- Decimal degrees coordinate display
- Hemispheric reference (N/S, E/W)
- Real-time timezone lookup via Geoapify
- Visual green notification on timezone detection
- Timezone dropdown auto-update

#### Timestamp Intelligence
- Three automatic modes based on photo source
- Browser timezone detection for camera photos
- Manual datetime entry for old photos
- UTC conversion with selected timezone
- Dual time display (original + UTC)

#### Image Manipulation
- Pinch-to-zoom on mobile (1x-5x)
- Click-drag pan when zoomed
- Touch gestures (single finger pan, pinch zoom)
- Rotation buttons (±45° increments)
- Double-tap/double-click reset
- Rotation-aware panning (moves correctly at any angle)

#### File Management
- Two-step save process on iOS (JSON then image)
- Web Share API integration for Files app
- Direct SharePoint upload with batch operation
- Structured JSON with all metadata fields
- GPS data always included (null if unavailable)

#### Developer Experience
- On-screen debug panel for iOS
- Real-time logging of operations
- Comprehensive error messages
- No build process required
- Single HTML file deployment
- CDN-based dependencies

### Technical Details
- Pure HTML/CSS/JavaScript
- Mobile-first responsive design
- PWA-ready (can be added to home screen)
- EXIF.js for metadata extraction
- MSAL.js for Microsoft authentication
- Geoapify API for timezone lookup
- Microsoft Graph API for SharePoint

### Browser Support
- iOS Safari 12+
- Android Chrome 80+
- Desktop Chrome, Edge, Firefox, Safari

### Known Issues
- Browser camera photos lack EXIF metadata (expected behavior)
- Timezone mapping requires pre-defined list (50+ included)
- Large files (>10MB) may be slow to upload to SharePoint
- Offline mode not supported

### Documentation
- README.md with full feature list
- SHAREPOINT_SETUP.md for Azure AD configuration
- TIMEZONE_DEBUG_GUIDE.md for troubleshooting
- CONTRIBUTING.md for contributors
- LICENSE file (MIT)

---

## Future Releases

See [ROADMAP](README.md#roadmap) in README.md for planned features.
