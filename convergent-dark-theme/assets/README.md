# Convergent Solutions Theme Assets

This directory contains placeholder files for all required brand assets. These placeholders must be replaced with official Convergent Solutions brand assets before production deployment.

## Required Assets

### Logo Files

#### logo.svg
- **Current Status**: ✅ Placeholder created
- **Format**: SVG (Scalable Vector Graphics)
- **Dimensions**: 400x100px viewBox (4:1 aspect ratio)
- **Purpose**: Primary logo for sidebar and header
- **Requirements**: 
  - Optimized for dark backgrounds
  - File size: < 20KB
  - Use brand colors

#### logo.png
- **Current Status**: ⚠️ Placeholder text file (needs actual PNG)
- **Format**: PNG with transparency
- **Dimensions**: 400x100px
- **Purpose**: Fallback logo for browsers without SVG support
- **Requirements**:
  - Optimized for dark backgrounds
  - File size: < 50KB
  - High quality (2x resolution recommended)

### Favicon Files

#### favicon.svg
- **Current Status**: ✅ Placeholder created
- **Format**: SVG
- **Dimensions**: 64x64px viewBox (scalable)
- **Purpose**: Modern browser favicon
- **Requirements**:
  - Simple, recognizable at small sizes
  - File size: < 5KB

#### favicon.png
- **Current Status**: ⚠️ Placeholder text file (needs actual PNG)
- **Format**: PNG
- **Dimensions**: 96x96px
- **Purpose**: Fallback favicon
- **Requirements**:
  - Clear at small sizes
  - File size: < 20KB

#### favicon.ico
- **Current Status**: ⚠️ Placeholder text file (needs actual ICO)
- **Format**: ICO (multi-size)
- **Sizes**: 16x16, 32x32, 48x48
- **Purpose**: Legacy browser support
- **Requirements**:
  - Include all three sizes
  - File size: < 30KB

### Mobile/Touch Icons

#### apple-touch-icon.png
- **Current Status**: ⚠️ Placeholder text file (needs actual PNG)
- **Format**: PNG
- **Dimensions**: 180x180px
- **Purpose**: iOS home screen icon
- **Requirements**:
  - No transparency (solid background)
  - File size: < 40KB
  - Optimized for iOS

### Splash Screens

#### splash-dark.png
- **Current Status**: ⚠️ Placeholder text file (needs actual PNG)
- **Format**: PNG
- **Dimensions**: 1920x1080px (16:9 aspect ratio)
- **Purpose**: Loading/splash screen for dark mode
- **Requirements**:
  - Convergent Solutions logo + tagline
  - Optimized for dark backgrounds
  - File size: < 200KB
  - Consider multiple resolutions for responsive design

## Asset Replacement Checklist

Before production deployment, ensure:

- [ ] All placeholder text files replaced with actual image files
- [ ] All images optimized for web (compressed)
- [ ] All images tested on dark backgrounds
- [ ] SVG files optimized (metadata removed, paths optimized)
- [ ] PNG files compressed (using pngquant or similar)
- [ ] ICO file includes all required sizes
- [ ] Total asset size < 500KB
- [ ] All assets approved by Convergent Solutions brand team

## Asset Optimization Tools

Recommended tools for asset optimization:

- **SVG**: [SVGO](https://github.com/svg/svgo) - `svgo --multipass logo.svg`
- **PNG**: [pngquant](https://pngquant.org/) - `pngquant --quality=65-80 logo.png`
- **ICO**: [ImageMagick](https://imagemagick.org/) - `convert favicon.png -define icon:auto-resize=16,32,48 favicon.ico`
- **General**: [ImageOptim](https://imageoptim.com/) (Mac) or [Squoosh](https://squoosh.app/) (Web)

## Testing Assets

After replacing placeholders:

1. Test logo visibility on dark backgrounds
2. Verify favicon displays correctly in browser tabs
3. Test apple-touch-icon on iOS devices
4. Verify splash screen loads correctly
5. Check total asset size is under 500KB
6. Validate all images are optimized

## Contact

For official Convergent Solutions brand assets, contact:
- Design Team: [design@convergentsolutions.com]
- Brand Guidelines: [Link to brand guidelines document]

---

**Last Updated**: 2026-01-16  
**Status**: Placeholders Created - Awaiting Official Assets
