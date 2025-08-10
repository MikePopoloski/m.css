# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

m.css is a no-nonsense, no-JavaScript CSS framework and documentation theme for content-oriented websites. It's designed for pure CSS/HTML with zero JavaScript dependencies.

## Key Components

### Core CSS Framework (`css/`)
- **Main themes**: `m-dark.css`, `m-light.css` - core theme files
- **Component library**: `m-components.css` - UI components
- **Layout system**: `m-grid.css`, `m-layout.css` - grid and layout utilities
- **Theme variants**: `m-theme-dark.css`, `m-theme-light.css` - theme customizations
- **Documentation styles**: `m-documentation.css` - styles for docs
- **Build system**: `postprocess.py` - CSS preprocessor that handles @import and CSS variables

### Documentation Tools (`documentation/`)
- **Doxygen theme**: `doxygen.py` - main Doxygen documentation generator
- **Python docs**: `python.py` - Python documentation generator
- **Search functionality**: `search.js`, `_search.py` - client and server-side search
- **Versioning system**: `versions.py`, `publish_version.py` - Material for MkDocs-style versioning
- **Templates**: `templates/doxygen/` and `templates/python/` - Jinja2 templates
- **Test suite**: Extensive test coverage in `test/` and `test_doxygen/`

### Pelican Theme (`pelican-theme/`)
- **Static blog theme**: Complete Pelican theme implementation
- **Templates**: Jinja2 templates for blog layouts
- **CSS assets**: Compiled CSS files for the theme

### Plugin System (`plugins/`)
- **Core plugins**: `m/` directory contains reusable Pelican plugins
- **Utility plugins**: `ansilexer.py`, `dot2svg.py`, `latex2svg.py` for content processing
- **Feature plugins**: Components, math rendering, image processing, etc.

## Common Development Commands

### CSS Development
```bash
# Build compiled CSS files
cd css/
./postprocess.py m-dark.css
./postprocess.py m-light.css

# Build documentation-specific CSS
./postprocess.py m-dark.css m-documentation.css -o m-dark+documentation.compiled.css
./postprocess.py m-light.css m-documentation.css -o m-light+documentation.compiled.css
```

### Testing
```bash
# Test Pelican theme
cd pelican-theme/
python -m unittest

# Test plugins
cd plugins/
python -m unittest

# Test documentation tools
cd documentation/
python -m unittest
node test/test-search.js

# Code coverage (requires coverage.py and istanbul)
cd documentation/
coverage run -m unittest && coverage html
npm install istanbul  # if not already installed
node ./node_modules/istanbul/lib/cli.js cover test/test-search.js
```

### Website Development
```bash
# Build and serve the project website (requires dependencies)
cd site/
pelican -Dlr  # Live reload server on http://localhost:8000
```

### Documentation Versioning
```bash
# Publish a new documentation version
cd documentation/
python3 publish_version.py v1.0.0 --config config.py --default --set-latest

# Manage versions
python3 versions.py list  # List all versions
python3 versions.py alias stable v1.0.0  # Set aliases
python3 versions.py add dev dev --warning "Development version"  # Add with warning
```

## Architecture Notes

### CSS Build System
- Uses custom `postprocess.py` to handle CSS @import statements and CSS variables
- Supports combining multiple CSS files into compiled versions
- Strips comments and optimizes output for production

### Documentation Generation
- **Doxygen integration**: Processes XML output from Doxygen into themed HTML
- **Python documentation**: Uses introspection to generate API docs
- **Search system**: Binary search index with JavaScript client
- **Template inheritance**: Jinja2-based templating with theme variants

### Plugin Architecture
- Pelican plugins follow standard plugin patterns
- Plugins are modular and can be used independently
- Test coverage includes both unit tests and integration tests with sample content

### Theme System
- Supports both dark and light themes
- CSS custom properties (variables) for easy customization
- Component-based architecture for reusable UI elements
- Responsive design without JavaScript

## Dependencies

### For Website Building
- Python 3.5+
- Pelican (static site generator)
- Pillow (image processing)
- matplotlib (for plots)
- Pyphen (hyphenation)
- TeXLive (for math rendering)

### For Testing
- Python unittest (built-in)
- Node.js (for JavaScript tests)
- coverage.py (for Python coverage)
- istanbul (for JavaScript coverage)