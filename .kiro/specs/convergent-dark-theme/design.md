# Convergent Solutions Dark Mode Theme - Design Document

## Design Overview

This document details the technical design for implementing a custom dark mode theme for Open WebUI with Convergent Solutions branding through CSS injection and Docker image customization.

**Design Approach**: CSS override strategy using Docker image extension
**Base Technology**: Open WebUI (SvelteKit + FastAPI)
**Deployment Method**: Docker container with injected custom.css

---

## Architecture

### System Architecture

```
┌─────────────────────────────────────────┐
│   Docker Container                      │
│  ┌───────────────────────────────────┐ │
│  │ Open WebUI Base Image             │ │
│  │ (ghcr.io/open-webui/open-webui)  │ │
│  └───────────────────────────────────┘ │
│              ↓                          │
│  ┌───────────────────────────────────┐ │
│  │ Custom Theme Layer                │ │
│  │ • /app/build/static/custom.css    │ │
│  │ • /app/build/static/favicon.*     │ │
│  │ • /app/build/static/logo.png      │ │
│  │ • /app/build/static/splash-*.png  │ │
│  └───────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

### File Structure

```
convergent-dark-theme/
├── Dockerfile                 # Docker image definition
├── custom.css                # Main theme stylesheet
├── assets/
│   ├── logo.svg              # Primary logo (SVG)
│   ├── logo.png              # Fallback logo (PNG)
│   ├── favicon.svg           # Favicon (SVG)
│   ├── favicon.png           # Favicon (PNG 96x96)
│   ├── favicon.ico           # Favicon (ICO multi-size)
│   ├── splash-dark.png       # Dark splash screen
│   └── apple-touch-icon.png  # iOS home screen icon
├── README.md                 # Installation and usage
├── CHANGELOG.md              # Version history
├── docker-compose.yml        # Docker Compose config
└── .dockerignore             # Build exclusions
```

---

## Color System Design

### Color Palette (Placeholder - Awaiting Brand Guidelines)

```css
:root {
  /* Primary Brand Colors - TO BE REPLACED */
  --cs-primary: #0066cc;        /* Convergent Solutions primary */
  --cs-secondary: #00cc99;      /* Convergent Solutions secondary */
  --cs-accent: #ff6b35;         /* Convergent Solutions accent */
  
  /* Dark Mode Base Colors */
  --bg-primary: #1a1a1a;        /* Main background */
  --bg-secondary: #2d2d2d;      /* Secondary background */
  --bg-tertiary: #3a3a3a;       /* Tertiary background */
  --bg-hover: #404040;          /* Hover state background */
  --bg-active: #4a4a4a;         /* Active state background */
  
  /* Text Colors */
  --text-primary: #ffffff;      /* Primary text (21:1 contrast) */
  --text-secondary: #b0b0b0;    /* Secondary text (7:1 contrast) */
  --text-tertiary: #808080;     /* Tertiary text (4.5:1 contrast) */
  --text-disabled: #606060;     /* Disabled text */
  
  /* Semantic Colors */
  --success: #4caf50;           /* Success states */
  --warning: #ff9800;           /* Warning states */
  --error: #f44336;             /* Error states */
  --info: #2196f3;              /* Info states */
  
  /* Border Colors */
  --border-primary: #404040;    /* Primary borders */
  --border-secondary: #333333;  /* Secondary borders */
  --border-focus: var(--cs-primary); /* Focus indicators */
  
  /* Shadow Colors */
  --shadow-sm: rgba(0, 0, 0, 0.2);
  --shadow-md: rgba(0, 0, 0, 0.3);
  --shadow-lg: rgba(0, 0, 0, 0.4);
  
  /* Spacing System */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  /* Typography */
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  
  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;
  
  /* Transitions */
  --transition-fast: 150ms ease;
  --transition-base: 200ms ease;
  --transition-slow: 300ms ease;
}
```

### Contrast Validation

All color combinations must meet WCAG 2.1 AA standards:
- Normal text (< 18pt): 4.5:1 minimum
- Large text (≥ 18pt): 3:1 minimum
- Interactive elements: 3:1 minimum

---

## Component Design

### 1. Sidebar Component

**Target Selectors**:
```css
#sidebar
#sidebar > div
#sidebar button
#sidebar a
```

**Design Specifications**:
- Background: `var(--bg-secondary)`
- Hover state: `var(--bg-hover)`
- Active item: `var(--cs-primary)` with 10% opacity background
- Border: 1px solid `var(--border-secondary)` on right edge
- Logo placement: Top of sidebar, centered, max-width 80%

### 2. Chat Interface

**Target Selectors**:
```css
.chat-container
.message-bubble
.user-message
.ai-message
.code-block
```

**Design Specifications**:
- User messages: `var(--bg-tertiary)` background
- AI messages: `var(--bg-secondary)` background
- Code blocks: Syntax highlighting with dark theme palette
- Message spacing: `var(--spacing-md)` between messages
- Border radius: `var(--radius-md)`

### 3. Buttons and Forms

**Target Selectors**:
```css
button[type="submit"]
#send-message-button
input[type="text"]
input[type="password"]
textarea
select
```

**Design Specifications**:
- Primary buttons: `var(--cs-primary)` background
- Secondary buttons: `var(--bg-tertiary)` background
- Hover: Lighten by 10%
- Focus: 2px solid `var(--border-focus)` outline
- Disabled: `var(--text-disabled)` with 50% opacity

### 4. Modals and Popovers

**Target Selectors**:
```css
.modal
.modal-backdrop
.tippy-box
.tippy-content
.popover
```

**Design Specifications**:
- Modal background: `var(--bg-primary)`
- Backdrop: rgba(0, 0, 0, 0.7)
- Tooltip background: `var(--cs-secondary)`
- Tooltip text: High contrast color
- Border radius: `var(--radius-lg)`
- Shadow: `var(--shadow-lg)`

---

## CSS Structure

### File Organization

```css
/* ============================================
   1. CSS CUSTOM PROPERTIES (VARIABLES)
   ============================================ */
:root { /* All variables defined here */ }

/* ============================================
   2. GLOBAL RESETS AND BASE STYLES
   ============================================ */
* { /* Global overrides */ }
body { /* Body styles */ }

/* ============================================
   3. LAYOUT COMPONENTS
   ============================================ */
/* 3.1 Sidebar */
/* 3.2 Main Content Area */
/* 3.3 Header */
/* 3.4 Footer */

/* ============================================
   4. INTERACTIVE ELEMENTS
   ============================================ */
/* 4.1 Buttons */
/* 4.2 Forms */
/* 4.3 Links */
/* 4.4 Toggle Switches */

/* ============================================
   5. CHAT INTERFACE
   ============================================ */
/* 5.1 Message Bubbles */
/* 5.2 Code Blocks */
/* 5.3 Input Area */

/* ============================================
   6. MODALS AND OVERLAYS
   ============================================ */
/* 6.1 Modals */
/* 6.2 Tooltips */
/* 6.3 Popovers */

/* ============================================
   7. RESPONSIVE DESIGN
   ============================================ */
@media (max-width: 768px) { /* Mobile */ }
@media (min-width: 769px) and (max-width: 1024px) { /* Tablet */ }
@media (min-width: 1025px) { /* Desktop */ }

/* ============================================
   8. ACCESSIBILITY ENHANCEMENTS
   ============================================ */
/* Focus indicators, high contrast mode, etc. */

/* ============================================
   9. ANIMATIONS AND TRANSITIONS
   ============================================ */
/* Smooth transitions, loading animations */
```

---

## Dockerfile Design

### Base Image Selection

**Version Pinning Strategy**:
- Use specific git commit tag (e.g., `git-49a928d`)
- Document base version in README and git tags
- Test theme compatibility before version updates

```dockerfile
# Pin to specific Open WebUI version
FROM ghcr.io/open-webui/open-webui:git-49a928d

# Metadata
LABEL maintainer="convergent-solutions@example.com"
LABEL description="Convergent Solutions branded Open WebUI"
LABEL version="1.0.0"
LABEL base-version="git-49a928d"

# Copy custom assets
COPY assets/favicon.svg /app/build/static/favicon.svg
COPY assets/favicon.png /app/build/static/favicon.png
COPY assets/favicon.ico /app/build/static/favicon.ico
COPY assets/logo.png /app/build/static/logo.png
COPY assets/splash-dark.png /app/build/static/splash-dark.png
COPY assets/apple-touch-icon.png /app/build/static/apple-touch-icon.png

# Copy custom CSS
COPY custom.css /app/build/static/custom.css

# Optional: Set environment variables
ENV WEBUI_NAME="Convergent Solutions AI"
ENV WEBUI_URL="https://ai.convergentsolutions.com"
```

---

## Asset Specifications

### Logo Requirements

**Primary Logo (logo.png)**:
- Format: PNG with transparency
- Dimensions: 400x100px (4:1 aspect ratio recommended)
- Optimized for dark backgrounds
- File size: < 50KB

**SVG Logo (logo.svg)**:
- Format: SVG (optimized)
- Viewbox: Maintain aspect ratio
- Colors: Use brand colors
- File size: < 20KB

### Favicon Requirements

**Favicon Formats**:
1. **favicon.svg**: Scalable vector (16x16 to 512x512)
2. **favicon.png**: 96x96px PNG
3. **favicon.ico**: Multi-size ICO (16x16, 32x32, 48x48)
4. **apple-touch-icon.png**: 180x180px for iOS

**Optimization**:
- SVG: Remove unnecessary metadata, optimize paths
- PNG: Use PNG-8 or PNG-24 with compression
- ICO: Include 16x16, 32x32, 48x48 sizes
- Total favicon assets: < 100KB

### Splash Screen Requirements

**splash-dark.png**:
- Dimensions: 1920x1080px (16:9 aspect ratio)
- Format: PNG with transparency or solid background
- Content: Convergent Solutions logo + tagline
- File size: < 200KB
- Optimization: Use appropriate compression

---

## Responsive Design Strategy

### Breakpoints

```css
/* Mobile First Approach */
/* Base styles: Mobile (< 768px) */

/* Tablet */
@media (min-width: 768px) {
  :root {
    --font-size-base: 1rem;
    --spacing-md: 1rem;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  :root {
    --font-size-base: 1rem;
    --spacing-md: 1.25rem;
  }
}

/* Large Desktop */
@media (min-width: 1440px) {
  :root {
    --font-size-base: 1.125rem;
    --spacing-md: 1.5rem;
  }
}
```

### Touch Target Sizing

- Minimum touch target: 44x44px (iOS/Android guidelines)
- Spacing between touch targets: 8px minimum
- Increase button padding on mobile devices

---

## Performance Optimization

### CSS Optimization

**Strategies**:
1. Use CSS custom properties for reusability
2. Minimize selector specificity where possible
3. Avoid expensive properties (box-shadow on large elements)
4. Use `will-change` sparingly for animations
5. Minify CSS for production

**Target Metrics**:
- CSS file size: < 100KB (unminified)
- CSS file size: < 30KB (minified + gzipped)
- First Paint: No delay from CSS
- Layout shifts: Zero CLS (Cumulative Layout Shift)

### Asset Optimization

**Image Optimization**:
- SVG: Remove metadata, optimize paths with SVGO
- PNG: Use pngquant or similar for compression
- ICO: Use optimal bit depth per size
- Total assets: < 500KB

**Loading Strategy**:
- Critical CSS: Inline if < 14KB
- Non-critical CSS: Load asynchronously
- Images: Use appropriate formats and compression
- Lazy load: Not applicable for theme assets

---

## Accessibility Design

### Focus Indicators

```css
*:focus-visible {
  outline: 2px solid var(--border-focus) !important;
  outline-offset: 2px !important;
  border-radius: var(--radius-sm) !important;
}

/* Remove default outline */
*:focus {
  outline: none !important;
}
```

### High Contrast Mode Support

```css
@media (prefers-contrast: high) {
  :root {
    --border-primary: #ffffff;
    --text-secondary: #ffffff;
    --bg-hover: #555555;
  }
}
```

### Reduced Motion Support

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Testing Strategy

### Browser Testing Matrix

| Browser | Version | Priority | Test Scope |
|---------|---------|----------|------------|
| Chrome | 90+ | High | Full testing |
| Firefox | 88+ | High | Full testing |
| Safari | 14+ | Medium | Core features |
| Edge | 90+ | Medium | Core features |
| iOS Safari | 14+ | High | Mobile testing |
| Chrome Mobile | Latest | High | Mobile testing |

### Testing Checklist

**Visual Testing**:
- [ ] All colors meet contrast requirements
- [ ] Logo displays correctly in all contexts
- [ ] Hover states are visible and smooth
- [ ] Focus indicators are clearly visible
- [ ] No layout shifts on page load
- [ ] Responsive design works at all breakpoints

**Functional Testing**:
- [ ] All buttons and links are clickable
- [ ] Forms are usable and styled correctly
- [ ] Modals and popovers display properly
- [ ] Keyboard navigation works throughout
- [ ] Screen reader announces elements correctly

**Performance Testing**:
- [ ] CSS file size under 100KB
- [ ] Total asset size under 500KB
- [ ] No performance degradation vs base Open WebUI
- [ ] Animations run at 60fps
- [ ] No excessive repaints/reflows

**Accessibility Testing**:
- [ ] WCAG 2.1 AA compliance verified
- [ ] Keyboard navigation fully functional
- [ ] Screen reader compatibility confirmed
- [ ] Color contrast ratios validated
- [ ] Focus indicators meet standards

---

## Implementation Phases

### Phase 1: Foundation (Week 1)
- Set up project structure
- Create Dockerfile with version pinning
- Define CSS custom properties
- Implement base color system
- Create placeholder logo assets

### Phase 2: Core Components (Week 2)
- Style sidebar component
- Style chat interface
- Style buttons and forms
- Implement hover and focus states
- Add responsive breakpoints

### Phase 3: Advanced Components (Week 3)
- Style modals and popovers
- Implement tooltips
- Add loading animations
- Style code blocks with syntax highlighting
- Implement accessibility features

### Phase 4: Integration & Testing (Week 4)
- Integrate real Convergent Solutions assets
- Apply brand colors throughout
- Comprehensive browser testing
- Accessibility audit and fixes
- Performance optimization

### Phase 5: Documentation & Deployment (Week 5)
- Write comprehensive README
- Create CHANGELOG
- Add screenshots and examples
- Build and tag Docker image
- Deploy to staging environment
- Stakeholder review and approval

---

## Dependencies

### Required Assets (From Convergent Solutions)
- [ ] Primary logo (SVG, PNG)
- [ ] Favicon (SVG, PNG, ICO)
- [ ] Brand color palette (hex codes)
- [ ] Brand guidelines document
- [ ] Splash screen design or approval for custom design

### Technical Dependencies
- [ ] Docker environment for building images
- [ ] Access to Docker registry for publishing
- [ ] Testing environment (staging server)
- [ ] Production deployment infrastructure
- [ ] SSL/TLS certificates for production

### Tool Dependencies
- [ ] CSS minification tool (cssnano, clean-css)
- [ ] Image optimization tools (SVGO, pngquant, ImageOptim)
- [ ] Accessibility testing tools (axe, WAVE)
- [ ] Contrast checker (WebAIM, Stark)
- [ ] Browser testing environment (BrowserStack or similar)

---

## Risk Mitigation

### Technical Risks

**Risk**: Open WebUI updates break theme
- **Mitigation**: Pin to specific version, test updates in staging
- **Contingency**: Maintain compatibility matrix, document breaking changes

**Risk**: CSS selectors change in Open WebUI updates
- **Mitigation**: Use broad selectors where appropriate, document selector dependencies
- **Contingency**: Quick-fix CSS updates, maintain selector mapping

**Risk**: Performance degradation from CSS overrides
- **Mitigation**: Optimize CSS, minimize selector complexity, test performance
- **Contingency**: Profile and optimize bottlenecks, remove non-critical styles

### Asset Risks

**Risk**: Logo assets not provided on time
- **Mitigation**: Request early, provide specifications, use placeholders
- **Contingency**: Use generic branding temporarily, deploy logo update separately

**Risk**: Brand colors fail accessibility standards
- **Mitigation**: Test colors early, provide alternatives, adjust as needed
- **Contingency**: Modify colors to meet standards, document changes

---

## Maintenance Plan

### Version Updates

**Semantic Versioning**:
- **MAJOR**: Breaking changes, Open WebUI base version updates
- **MINOR**: New features, significant style improvements
- **PATCH**: Bug fixes, minor style adjustments

**Update Process**:
1. Check for Open WebUI updates monthly
2. Test theme compatibility in staging
3. Update CSS if needed for compatibility
4. Update documentation and CHANGELOG
5. Tag new version and deploy

### Monitoring

**Metrics to Track**:
- CSS file size over time
- Page load performance
- User feedback on usability
- Accessibility compliance
- Browser compatibility issues

**Review Schedule**:
- Weekly: User feedback review
- Monthly: Performance metrics review
- Quarterly: Accessibility audit
- Annually: Comprehensive theme review

---

## Documentation Requirements

### README.md Contents
1. Project overview and features
2. Installation instructions (Docker, Docker Compose)
3. Configuration options
4. Customization guide
5. Troubleshooting section
6. License and credits

### CHANGELOG.md Contents
1. Version history with dates
2. Added features
3. Changed styles
4. Fixed bugs
5. Breaking changes
6. Base Open WebUI version updates

### Additional Documentation
- Screenshots (desktop, mobile, tablet)
- Before/after comparisons
- Color palette reference
- Customization examples
- Deployment guide

---

## Success Metrics

### Quantitative Metrics
- CSS file size: < 100KB ✓
- Total asset size: < 500KB ✓
- Contrast ratios: All ≥ 4.5:1 ✓
- Browser compatibility: 100% on target browsers ✓
- Performance: No degradation vs base ✓

### Qualitative Metrics
- Stakeholder approval ✓
- User satisfaction feedback ✓
- Accessibility compliance ✓
- Visual consistency ✓
- Brand alignment ✓

---

## Approval

This design document requires approval from:
- [ ] Convergent Solutions IT Lead
- [ ] Project Stakeholder
- [ ] Development Team Lead
- [ ] UX/Design Team (if applicable)

**Version**: 1.0.0  
**Date**: 2026-01-16  
**Status**: Draft - Pending Review  
**Dependencies**: Requires approved requirements.md

---

## Next Steps

After design approval:
1. Create tasks.md with detailed implementation tasks
2. Obtain Convergent Solutions brand assets
3. Set up development environment
4. Begin Phase 1 implementation
5. Schedule regular stakeholder check-ins
