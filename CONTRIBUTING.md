# Contributing to Water Meter Photo Logger

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## 🐛 Bug Reports

If you find a bug, please open an issue with:
- Clear description of the problem
- Steps to reproduce
- Expected vs actual behavior
- Screenshots (especially from mobile devices)
- Browser/OS information
- Debug console output (if visible)

## ✨ Feature Requests

Feature requests are welcome! Please include:
- Clear description of the feature
- Use case / why it's needed
- Any implementation ideas you have

## 🔧 Pull Requests

### Before You Start

1. Check existing issues and PRs to avoid duplicates
2. For major changes, open an issue first to discuss
3. Test on both desktop and mobile (iOS Safari and Android Chrome minimum)

### Development Process

1. **Fork the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/water-meter-logger.git
   cd water-meter-logger
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Edit `exif-extractor.html` directly (no build process)
   - Test in browser (both desktop and mobile)
   - Add comments for complex logic
   - Follow existing code style

4. **Test thoroughly**
   - Test with photos that have GPS data
   - Test with photos without GPS data
   - Test with browser camera captures
   - Test timezone detection
   - Test file saving (both local and SharePoint if configured)
   - Verify mobile responsiveness

5. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add feature: description"
   ```

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Open a Pull Request**
   - Provide clear description of changes
   - Reference any related issues
   - Include screenshots for UI changes
   - Mention if you tested on mobile

### Code Style

- Use clear, descriptive variable names
- Add comments for complex logic
- Keep functions focused and under 50 lines when possible
- Use ES6+ JavaScript features (arrow functions, const/let, template literals)
- Follow existing formatting (indentation, spacing)

### Common Contributions

#### Adding New Timezones

1. Find the `getUTCOffsetFromTimezone()` function (around line 1650)
2. Add to the `timezoneMap` object:
   ```javascript
   'Your/Timezone/Name': 'UTC±HH:MM',
   ```
3. Test with a photo from that location
4. Submit PR with timezone name and test results

#### Improving Mobile UX

- Test on physical devices (not just browser dev tools)
- Consider touch targets (minimum 44x44px)
- Test with slow networks
- Verify keyboard doesn't obscure inputs
- Check orientation changes

#### Adding New Units

1. Find the units dropdown in HTML (around line 620)
2. Add your unit to the `<select>` options
3. Update README.md with new unit
4. Submit PR

### Testing Checklist

Before submitting a PR, verify:

- [ ] Works in iOS Safari
- [ ] Works in Android Chrome
- [ ] Works in desktop Chrome/Edge/Firefox
- [ ] GPS detection works (if modified)
- [ ] Timezone detection works (if modified)
- [ ] File saving works (both local and SharePoint)
- [ ] Image rotation/zoom/pan works
- [ ] Form validation works
- [ ] No console errors
- [ ] Mobile responsive (test at 375px width minimum)
- [ ] Debug panel still works

## 📝 Documentation

Documentation improvements are always welcome:
- Fix typos
- Clarify instructions
- Add examples
- Update screenshots
- Expand troubleshooting section

## 🔒 Security

If you discover a security vulnerability, please email privately rather than opening a public issue.

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

## ❓ Questions?

Feel free to open an issue with the "question" label if you need help or clarification.

---

**Thank you for contributing!** 🎉
