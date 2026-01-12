# TripCoil Terminal V1.1

**A professional-grade browser-based terminal for electrical wiring trace verification and documentation**

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Development Standards](#development-standards)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Documentation Standards](#documentation-standards)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

TripCoil Terminal V1.1 is a single-file HTML application designed for field technicians and electrical engineers to verify, trace, and document electrical wiring connections in real-time. Built following industry best practices and modern web standards, it provides an intuitive terminal-style interface for managing complex wiring diagrams.

**Live Demo:** Open the HTML file in any modern browser - no installation required.

---

## ✨ Features

### Core Modules

#### 1. **Trace Tool** (Active Verification)
- Real-time wire verification workflow
- Bidirectional wire matching (forward/reverse)
- Status tracking: Confirmed (C) / Unconfirmed (U) / Pending (?)
- Automatic discovery stack management
- Circle-back navigation for incomplete traces
- Undo functionality with 50-state history buffer

#### 2. **Plotter** (Schematic Generator)
- ASCII tree-based wiring diagrams
- Visual status indicators (✓ confirmed, ✗ unconfirmed, ? pending)
- Hierarchical display with root-node detection
- Print-friendly output with custom styling
- Copy-to-clipboard support

#### 3. **Data Interface** (CSV Import/Export)
- Standards-compliant CSV parsing (14-column format)
- Bidirectional data sync with live trace state
- Validation warnings for malformed data
- Timestamped file exports
- Auto-fix for common formatting issues

### User Experience

- **Retro Terminal Aesthetic**: High-contrast neon-on-dark theme
- **Mobile-Ready**: Virtual history/undo buttons for touch devices
- **Keyboard Shortcuts**: 
  - `Ctrl+S` - Save CSV
  - `Ctrl+O` - Load CSV
  - `Ctrl+L` - Clear screen
  - `↑/↓` - Command history navigation
  - `Esc` - Close overlays
- **Command History**: Persistent navigation through previous inputs
- **Unsaved Data Protection**: Browser warning before accidental closure

---

## 🏗️ Development Standards

This project was built following **Google's Engineering Best Practices** and modern web development standards:

### Code Organization

**Separation of Concerns** ([Google's SRP](https://google.github.io/eng-practices/review/reviewer/looking-for.html))
```
StateManager      → Application state & history
UIRenderer        → DOM manipulation & display
CSVParser         → Data parsing & validation
TraceTool         → Wire verification logic
MenuTool          → Navigation interface
Plotter           → Diagram generation
DataInterface     → File operations
GlobalCommands    → System-level commands
```

### Code Quality Standards

1. **Constants Over Magic Strings**
   ```javascript
   const STATUS = { CONFIRMED: 'C', UNCONFIRMED: 'U', ... };
   const PHASE = { SEED_PANEL: 'SEED_PANEL', ... };
   ```

2. **Defensive Programming**
   - Input sanitization (XSS prevention)
   - CSV validation before processing
   - Error boundaries with try/catch
   - Null-safe operations with fallbacks

3. **Performance Optimization**
   - Debounced scroll operations
   - Limited undo stack (prevents memory bloat)
   - Normalized key lookups (O(1) comparisons)
   - Efficient DOM updates

4. **Strict Mode**
   ```javascript
   'use strict'; // Catches common coding errors
   ```

### Documentation Standards

Following [GitHub's best practices](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes):

- **JSDoc-style function comments** for all public methods
- **Inline comments** explaining complex logic
- **Descriptive variable names** (no abbreviations like `cp`, `cd`)
- **Modular architecture** for easy code navigation

---

## 🏛️ Architecture

### Data Flow

```
User Input → StateManager → Tool Handler → CSVParser/UIRenderer
                ↓
        TraceTool.rows (Single Source of Truth)
                ↓
        Export → CSV/Plotter
```

### State Management

- **Single Source of Truth**: `TraceTool.rows` contains all wire data
- **Immutable Updates**: State changes create new snapshots for undo
- **Phase-Based Flow**: Finite state machine prevents invalid transitions

### CSV Format (14-Column Standard)

```csv
ROW_TYPE,PANEL,DEVICE_TAG,TERMINAL,TERM_KIND,ELEM_ID,DEVICE_PART,
TO_PANEL,TO_DEVICE,TO_TERMINAL,CABLE_ID,STATUS,SIGNAL_ID,REMARKS
```

**Example:**
```csv
WIR,1,AA,A01,,,,2,BB,B01,,C,,
WIR,2,BB,B01,,,,3,CC,C01,,U,,
```

**Status Codes:**
- `C` - Confirmed (physically verified)
- `U` - Unconfirmed (wire found but not verified)
- `?` - Pending (not yet traced)

---

## 💾 Installation

### Method 1: Direct Download
1. Download `tripcoil-terminal.html`
2. Open in any modern browser (Chrome, Firefox, Safari, Edge)
3. Start tracing!

### Method 2: Serve Locally
```bash
# Python 3
python -m http.server 8000

# Node.js
npx http-server
```

Then navigate to `http://localhost:8000/tripcoil-terminal.html`

### Requirements
- Modern browser with ES6+ support
- JavaScript enabled
- 1MB+ free RAM

---

## 📖 Usage

### Quick Start

1. **Launch the application** - Open the HTML file
2. **Select "1" or type "TRACE"** - Enter Trace Tool
3. **Follow the prompts:**
   ```
   FROM PANEL? → 1
   FROM DEVICE? → AA
   FROM TERMINAL? → A01
   TO PANEL? → 2
   TO DEVICE? → BB
   TO TERMINAL? → B01
   FOUND WIRE 1/AA/A01? (Y/N) → Y
   WIRE COUNT? → 0
   ```
4. **Export your data:**
   - Type `CSV` or `EXPORT` to view/save
   - Type `PLOT` to generate diagram

### Advanced Workflows

#### Circle-Back Tracing
When you specify a wire count > 0 at a destination, the system automatically:
1. Adds that terminal to the discovery stack
2. Navigates to that terminal after current trace completes
3. Reminds you of pending wires with countdown

#### Undo/Redo
- Press `↑` (Up Arrow) to undo last input
- Mobile users: Tap `∧` button
- Up to 50 actions can be undone

#### Batch Import
1. Prepare CSV with existing wiring data
2. Type `LOAD` or press `Ctrl+O`
3. Select your file
4. System validates and imports automatically

---

## 📚 Documentation Standards

This project follows **best practices for repository documentation** as outlined in [GitHub's documentation guide](https://docs.github.com/en/repositories/creating-and-managing-repositories/best-practices-for-repositories):

### README Structure
✅ What the project does (Overview)  
✅ Why it's useful (Features)  
✅ How to get started (Installation)  
✅ Where to get help (Usage)  
✅ Who maintains it (Contributing section below)

### Code Documentation
- **Every function** has purpose documentation
- **Complex logic** includes step-by-step comments
- **Constants** are grouped and explained
- **Error messages** are user-friendly and actionable

### Change Documentation
Following the principle of **"document immediately after code changes"**, all features include:
- Inline comments explaining the "why"
- Updated function signatures
- Version notes in commit messages

---

## 🤝 Contributing

### Code Standards
1. **Use `'use strict';`** at the top of script blocks
2. **Follow the modular pattern** - add new features as separate objects
3. **Sanitize user input** using the `sanitize()` helper
4. **Add JSDoc comments** for new public functions
5. **Update this README** with new features

### Adding Features

**Example: Adding a new command**
```javascript
// In GlobalCommands.handle()
if (normalizedCmd === 'MYNEWCOMMAND') {
  MyNewFeature.execute();
  return true;
}

// Create new module
const MyNewFeature = {
  execute() {
    UIRenderer.appendLine('<span class="info">Feature activated!</span>');
    // Your logic here
  }
};
```

### Testing Guidelines
1. Test with malformed CSV data
2. Verify mobile button functionality
3. Check keyboard shortcuts across browsers
4. Validate undo/redo for edge cases
5. Ensure accessibility (keyboard navigation)

---

## 🔒 Security

- **XSS Protection**: All user input is sanitized before rendering
- **No External Dependencies**: Zero supply chain risk
- **Client-Side Only**: No data leaves your browser
- **No Cookies/Tracking**: Complete privacy

---

## 📊 Technical Specifications

| Feature | Specification |
|---------|---------------|
| **Lines of Code** | ~1,200 (excluding comments/whitespace) |
| **Dependencies** | None (vanilla JavaScript) |
| **Browser Support** | Chrome 60+, Firefox 55+, Safari 11+, Edge 79+ |
| **File Size** | ~45KB (unminified) |
| **Performance** | Handles 1000+ wire traces without lag |
| **Undo Buffer** | 50 states (~5KB memory) |

---

## 🎨 Design Philosophy

This project prioritizes:
1. **Zero Configuration** - Works immediately out of the box
2. **Single File Distribution** - No build process required
3. **Offline-First** - All functionality works without internet
4. **Keyboard-Centric** - Designed for rapid data entry
5. **Print-Ready Output** - Professional documentation export

---

## 📄 License

This project is provided as-is for educational and professional use. Modify freely for your organization's needs.

---

## 🙏 Acknowledgments

Built with modern web standards and inspired by:
- **Google's Engineering Practices** - Code review standards
- **GitHub's Documentation Guide** - README structure
- **Retro Terminal Aesthetics** - VT100/ANSI art styling

---

## 📞 Support

For issues or feature requests:
1. Review the [Usage](#usage) section
2. Check inline help with `HELP` command
3. Inspect browser console for error messages
4. Verify CSV format matches [specification](#csv-format-14-column-standard)

---

**Version:** 1.1  
**Last Updated:** 2026-01-12  
**Maintained By:** Open Source Community

---

*This README follows [Google's documentation standards](https://google.github.io/styleguide/docguide/READMEs.html) and GitHub's best practices for repository documentation.*