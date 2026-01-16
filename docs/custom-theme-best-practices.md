---
inclusion: always
---

# Open WebUI Custom Theme Best Practices

This steering guide provides best practices for creating, maintaining, and deploying custom themes for Open WebUI through CSS injection.

## Overview

Open WebUI doesn't have built-in theming support, but you can customize its appearance by injecting custom CSS files into the Docker image. This approach allows complete visual customization while maintaining core functionality.

## Core Principles

### Version Pinning
- **ALWAYS use specific version tags** (e.g., `git-49a928d`) instead of `main` or `latest`
- Prevents breaking changes from upstream updates
- Ensures theme compatibility and stability
- Check [Open WebUI releases](https://github.com/open-webui/open-webui/releases) for available tags
- Use CUDA or Ollama-specific tags if needed (e.g., `git-49a928d-cuda`)

### CSS Override Strategy
- Use `!important` to override existing styles
- Define CSS custom properties (variables) for consistency
- Keep selectors specific but maintainable
- Document complex selectors with comments

### Testing Requirements
- Test theme in both light and dark modes (if applicable)
- Verify responsive design on mobile, tablet, and desktop
- Check accessibility (contrast ratios, readability)
- Test all major UI components before deployment

## Project Structure

### Recommended File Organization
```
open-webui-theme/
├── Dockerfile              # Docker image definition
├── custom.css             # Main theme stylesheet
├── favicon.svg            # Optional: Custom favicon (SVG)
├── favicon.png            # Optional: Custom favicon (PNG)
├── favicon.ico            # Optional: Custom favicon (ICO)
├── README.md              # Theme documentation
├── .dockerignore          # Docker build exclusions
└── examples/              # Optional: Theme variations
    ├── blue-theme.css
    ├── dark-theme.css
    └── corporate-theme.css
```

## Dockerfile Best Practices

### Basic Template
```dockerfile
# Use specific version tag - NEVER use 'main' or 'latest'
FROM ghcr.io/open-webui/open-webui:git-49a928d

# Metadata for documentation
LABEL maintainer="your-email@example.com"
LABEL description="Custom themed Open WebUI"
LABEL version="1.0.0"

# Optional: Replace favicon icons
COPY favicon.svg /app/build/static/favicon.svg
COPY favicon.png /app/build/static/favicon.png
COPY favicon.ico /app/build/static/favicon.ico

# Copy custom CSS file
COPY custom.css /app/build/static/custom.css

# Optional: Set environment variables
ENV WEBUI_NAME="Custom Open WebUI"
```

### Multi-Stage Builds (Advanced)
```dockerfile
# Stage 1: Prepare assets
FROM alpine:latest AS assets
WORKDIR /assets
COPY custom.css .
COPY favicon.* .
# Optional: Minify CSS, optimize images, etc.

# Stage 2: Build final image
FROM ghcr.io/open-webui/open-webui:git-49a928d
COPY --from=assets /assets/custom.css /app/build/static/custom.css
COPY --from=assets /assets/favicon.* /app/build/static/
```

### Version-Specific Tags
```dockerfile
# For CUDA support
FROM ghcr.io/open-webui/open-webui:git-49a928d-cuda

# For bundled Ollama
FROM ghcr.io/open-webui/open-webui:git-49a928d-ollama

# For specific architecture
FROM ghcr.io/open-webui/open-webui:git-49a928d-amd64
```

## CSS Best Practices

### Use CSS Custom Properties
```css
:root {
  /* Color palette */
  --primary-color: #00487d;
  --secondary-color: #ffd600;
  --background-color: #e2eef5;
  --hover-color: #d4e3ed;
  --text-color: #1a1a1a;
  --border-color: #cccccc;
  
  /* Spacing */
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  
  /* Typography */
  --font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-size-base: 16px;
  --font-size-sm: 14px;
  --font-size-lg: 18px;
  
  /* Borders and shadows */
  --border-radius: 8px;
  --box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
```

### Organized CSS Structure
```css
/* ============================================
   GLOBAL STYLES
   ============================================ */

/* Base text color override */
* {
  color: var(--text-color) !important;
}

/* ============================================
   LAYOUT COMPONENTS
   ============================================ */

/* Sidebar styling */
#sidebar {
  background-color: var(--background-color) !important;
  border-right: 1px solid var(--border-color) !important;
}

#sidebar > div *:hover {
  background-color: var(--hover-color) !important;
  border-radius: var(--border-radius) !important;
}

/* ============================================
   INTERACTIVE ELEMENTS
   ============================================ */

/* Primary buttons */
#send-message-button,
button[type="submit"] {
  background-color: var(--secondary-color) !important;
  color: var(--text-color) !important;
  border-radius: var(--border-radius) !important;
  transition: all 0.2s ease !important;
}

#send-message-button:hover,
button[type="submit"]:hover {
  opacity: 0.9 !important;
  transform: translateY(-1px) !important;
}

/* Toggle switches */
[role="switch"][aria-checked="true"] {
  background-color: var(--secondary-color) !important;
}

/* ============================================
   TOOLTIPS AND POPOVERS
   ============================================ */

.tippy-content,
.tippy-box {
  background-color: var(--secondary-color) !important;
  border-color: var(--secondary-color) !important;
  color: var(--text-color) !important;
}

/* ============================================
   VOICE AND SPECIAL FEATURES
   ============================================ */

[aria-label="Voice mode"] {
  background-color: var(--secondary-color) !important;
}

/* ============================================
   RESPONSIVE DESIGN
   ============================================ */

@media (max-width: 768px) {
  :root {
    --font-size-base: 14px;
    --spacing-md: 0.75rem;
  }
}

/* ============================================
   ACCESSIBILITY ENHANCEMENTS
   ============================================ */

/* Focus indicators */
*:focus-visible {
  outline: 2px solid var(--primary-color) !important;
  outline-offset: 2px !important;
}

/* High contrast mode support */
@media (prefers-contrast: high) {
  :root {
    --border-color: #000000;
  }
}
```

### Finding CSS Selectors

**Process**:
1. Open Open WebUI in browser
2. Right-click element → "Inspect" or "Inspect Element"
3. Find the CSS selector in DevTools
4. Test selector in browser console: `document.querySelector('your-selector')`
5. Add to custom.css with `!important`

**Selector Priority** (from most to least specific):
```css
/* Most specific - use sparingly */
#specific-id.class-name[attribute="value"] { }

/* Good balance of specificity */
#sidebar > div.item { }

/* Less specific - may need !important */
.button { }

/* Least specific - definitely needs !important */
button { }
```

## Color Scheme Guidelines

### Accessibility Requirements
- **Contrast ratio**: Minimum 4.5:1 for normal text, 3:1 for large text
- **Color blindness**: Test with color blindness simulators
- **Dark mode**: Consider providing dark mode variant
- **Tools**: Use WebAIM Contrast Checker, Coolors, Adobe Color

### Color Palette Structure
```css
:root {
  /* Primary brand colors */
  --brand-primary: #00487d;
  --brand-secondary: #ffd600;
  --brand-accent: #ff6b6b;
  
  /* Semantic colors */
  --success: #51cf66;
  --warning: #ffd43b;
  --error: #ff6b6b;
  --info: #339af0;
  
  /* Neutral colors */
  --gray-50: #f8f9fa;
  --gray-100: #e9ecef;
  --gray-200: #dee2e6;
  --gray-300: #ced4da;
  --gray-400: #adb5bd;
  --gray-500: #868e96;
  --gray-600: #495057;
  --gray-700: #343a40;
  --gray-800: #212529;
  --gray-900: #0d0f12;
}
```

### Dark Mode Support
```css
/* Light mode (default) */
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --text-primary: #212529;
  --text-secondary: #495057;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #212529;
    --bg-secondary: #343a40;
    --text-primary: #f8f9fa;
    --text-secondary: #adb5bd;
  }
}

/* Manual dark mode class */
[data-theme="dark"] {
  --bg-primary: #212529;
  --bg-secondary: #343a40;
  --text-primary: #f8f9fa;
  --text-secondary: #adb5bd;
}
```

## Build and Deployment

### Building the Image
```bash
# Build with tag
docker build -t my-org/open-webui-custom:1.0.0 .

# Build with multiple tags
docker build -t my-org/open-webui-custom:1.0.0 \
             -t my-org/open-webui-custom:latest .

# Build for specific platform
docker build --platform linux/amd64 -t my-org/open-webui-custom:1.0.0 .

# Build with build args
docker build --build-arg BASE_VERSION=git-49a928d \
             -t my-org/open-webui-custom:1.0.0 .
```

### Running the Custom Image
```bash
# Basic run
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  --name open-webui-custom \
  my-org/open-webui-custom:1.0.0

# With environment variables
docker run -d -p 3000:8080 \
  -v open-webui:/app/backend/data \
  -e WEBUI_NAME="Custom Open WebUI" \
  -e OLLAMA_BASE_URL=http://ollama:11434 \
  --name open-webui-custom \
  my-org/open-webui-custom:1.0.0

# With GPU support (CUDA base image required)
docker run -d -p 3000:8080 \
  --gpus all \
  -v open-webui:/app/backend/data \
  --name open-webui-custom \
  my-org/open-webui-custom:1.0.0-cuda
```

### Docker Compose
```yaml
version: '3.8'

services:
  open-webui-custom:
    build:
      context: .
      dockerfile: Dockerfile
    image: my-org/open-webui-custom:1.0.0
    container_name: open-webui-custom
    ports:
      - "3000:8080"
    volumes:
      - open-webui:/app/backend/data
    environment:
      - WEBUI_NAME=Custom Open WebUI
      - OLLAMA_BASE_URL=http://ollama:11434
    restart: unless-stopped
    depends_on:
      - ollama

  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped

volumes:
  open-webui:
  ollama:
```

## Testing Checklist

### Pre-Deployment Testing
- [ ] Build completes without errors
- [ ] Container starts successfully
- [ ] All pages load correctly
- [ ] CSS applies to all targeted elements
- [ ] No console errors in browser DevTools
- [ ] Responsive design works on mobile/tablet/desktop
- [ ] Accessibility: Contrast ratios meet WCAG standards
- [ ] Accessibility: Keyboard navigation works
- [ ] Accessibility: Screen reader compatibility
- [ ] Dark mode (if implemented) works correctly
- [ ] All interactive elements (buttons, forms) are styled
- [ ] Tooltips and popovers are readable
- [ ] Icons and favicons display correctly

### Browser Compatibility
Test in:
- [ ] Chrome/Chromium
- [ ] Firefox
- [ ] Safari (if applicable)
- [ ] Edge
- [ ] Mobile browsers (iOS Safari, Chrome Mobile)

### Performance Testing
- [ ] CSS file size is reasonable (< 100KB recommended)
- [ ] No layout shifts on page load
- [ ] Animations are smooth (60fps)
- [ ] No excessive repaints/reflows

## Maintenance and Updates

### Version Management
```bash
# Tag format: v{major}.{minor}.{patch}-base-{open-webui-version}
# Example: v1.0.0-base-git-49a928d

git tag -a v1.0.0-base-git-49a928d -m "Initial theme release"
git push origin v1.0.0-base-git-49a928d
```

### Updating Base Image
```bash
# 1. Check for new Open WebUI releases
# Visit: https://github.com/open-webui/open-webui/releases

# 2. Update Dockerfile with new version tag
# FROM ghcr.io/open-webui/open-webui:git-NEW-VERSION

# 3. Test theme with new version
docker build -t my-org/open-webui-custom:test .
docker run -d -p 3000:8080 my-org/open-webui-custom:test

# 4. Run full testing checklist

# 5. Update CSS if needed for compatibility

# 6. Tag and release new version
git tag -a v1.1.0-base-git-NEW-VERSION -m "Update base image"
```

### Changelog Maintenance
Keep a CHANGELOG.md documenting:
- Base Open WebUI version updates
- CSS changes and improvements
- Bug fixes
- Breaking changes
- Migration instructions

## Common Pitfalls

### ❌ Avoid These Mistakes

**Using `main` or `latest` tags**:
```dockerfile
# BAD - will break with updates
FROM ghcr.io/open-webui/open-webui:main

# GOOD - stable and predictable
FROM ghcr.io/open-webui/open-webui:git-49a928d
```

**Overly broad selectors**:
```css
/* BAD - affects everything */
* {
  background-color: blue !important;
}

/* GOOD - targeted styling */
#sidebar {
  background-color: blue !important;
}
```

**Missing `!important`**:
```css
/* BAD - may not override */
button {
  background-color: yellow;
}

/* GOOD - ensures override */
button {
  background-color: yellow !important;
}
```

**Hardcoded colors**:
```css
/* BAD - hard to maintain */
#sidebar { background-color: #00487d !important; }
button { background-color: #00487d !important; }

/* GOOD - use variables */
:root { --primary: #00487d; }
#sidebar { background-color: var(--primary) !important; }
button { background-color: var(--primary) !important; }
```

**No accessibility consideration**:
```css
/* BAD - poor contrast */
:root {
  --text-color: #cccccc;
  --bg-color: #ffffff;
}

/* GOOD - sufficient contrast */
:root {
  --text-color: #212529;
  --bg-color: #ffffff;
}
```

## Documentation Requirements

### README.md Template
```markdown
# Custom Open WebUI Theme

Brief description of your theme.

## Preview
![Theme Screenshot](screenshot.png)

## Features
- Custom color scheme
- Enhanced accessibility
- Responsive design
- Dark mode support

## Installation

### Using Docker
\`\`\`bash
docker build -t my-custom-theme .
docker run -d -p 3000:8080 -v open-webui:/app/backend/data my-custom-theme
\`\`\`

### Using Docker Compose
\`\`\`bash
docker-compose up -d
\`\`\`

## Configuration
- Base Image: `ghcr.io/open-webui/open-webui:git-49a928d`
- Tested with: Open WebUI v0.7.2

## Customization
Edit `custom.css` to modify colors and styles.

## License
MIT License

## Credits
Based on [Open WebUI](https://github.com/open-webui/open-webui)
```

### Include Screenshots
- Full page screenshots
- Before/after comparisons
- Mobile and desktop views
- Dark mode (if applicable)

## Security Considerations

### Image Security
- Use official Open WebUI base images only
- Scan images for vulnerabilities: `docker scan my-org/open-webui-custom:1.0.0`
- Keep base image updated with security patches
- Don't include sensitive data in CSS or Dockerfile

### Production Deployment
- Use specific version tags (never `latest`)
- Implement proper access controls
- Use HTTPS/TLS for production
- Regular security audits
- Monitor for upstream security advisories

## Resources

### Official Documentation
- [Open WebUI GitHub](https://github.com/open-webui/open-webui)
- [Open WebUI Documentation](https://docs.openwebui.com/)
- [Sliplane Theme Guide](https://sliplane.io/blog/how-to-build-custom-open-webui-themes)
- [Sliplane Theme Repository](https://github.com/sliplane/open-webui-theme)

### CSS and Design Tools
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Coolors Color Palette Generator](https://coolors.co/)
- [Adobe Color](https://color.adobe.com/)
- [CSS Custom Properties (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)

### Docker Resources
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

---

*Remember: All normal Open WebUI configuration still applies when using custom themes. The theme only changes the visual appearance, not the core functionality.*
