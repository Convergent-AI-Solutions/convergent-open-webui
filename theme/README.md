# Open WebUI Dark Theme

A professional dark theme for Open WebUI with custom branding and enhanced user experience.

## Features

- 🌙 **Modern Dark Design** - Easy on the eyes with carefully selected colors
- 🎨 **Custom Color Palette** - Indigo and purple accent colors
- ♿ **Accessible** - WCAG compliant contrast ratios
- 📱 **Responsive** - Works seamlessly on desktop, tablet, and mobile
- ⚡ **Smooth Animations** - Polished transitions and interactions
- 🎯 **Custom Branding** - Replace logos with your own

## Preview

_Screenshots will be added once the theme is deployed_

## Prerequisites

- Docker and Docker Compose installed
- (Optional) Ollama for local LLM support
- Your custom logo files (SVG, PNG, ICO formats)

## Installation

### Step 1: Add Your Logos

Replace the placeholder favicon files with your custom logos:

```bash
# Copy your logo files to the theme directory
cp /path/to/your/logo.svg theme/favicon.svg
cp /path/to/your/logo.png theme/favicon.png
cp /path/to/your/logo.ico theme/favicon.ico
```

**Logo Requirements:**
- `favicon.svg` - Vector format, recommended size: 32x32 or scalable
- `favicon.png` - PNG format, recommended size: 192x192 or 512x512
- `favicon.ico` - ICO format, recommended size: 16x16, 32x32, 48x48

### Step 2: Build and Run

#### Using Docker Compose (Recommended)

```bash
# Navigate to theme directory
cd theme

# Build and start services
docker-compose up -d

# View logs
docker-compose logs -f open-webui-dark
```

#### Using Docker Build

```bash
# Navigate to theme directory
cd theme

# Build the image
docker build -t open-webui-dark:1.0.0 .

# Run the container
docker run -d -p 3000:8080 \
  -v open-webui-data:/app/backend/data \
  --name open-webui-dark \
  open-webui-dark:1.0.0
```

### Step 3: Access Your Themed Open WebUI

Open your browser and navigate to:
- **Local**: http://localhost:3000
- **Network**: http://YOUR_IP:3000

## Configuration

### Environment Variables

Edit `docker-compose.yml` to customize:

```yaml
environment:
  - WEBUI_NAME=Your Custom Name
  - OLLAMA_BASE_URL=http://ollama:11434
  - OPENAI_API_KEY=your-key-here  # If using OpenAI
```

### Color Customization

Edit `custom.css` to change colors:

```css
:root {
  --accent-primary: #6366f1;      /* Change primary accent color */
  --accent-secondary: #8b5cf6;    /* Change secondary accent color */
  --primary-bg: #1a1a1a;          /* Change main background */
  --secondary-bg: #2d2d2d;        /* Change secondary background */
}
```

## Updating

### Update Base Image

1. Check for new Open WebUI releases: https://github.com/open-webui/open-webui/releases
2. Update the `FROM` line in `Dockerfile`:
   ```dockerfile
   FROM ghcr.io/open-webui/open-webui:git-NEW-VERSION
   ```
3. Rebuild and test:
   ```bash
   docker-compose down
   docker-compose build --no-cache
   docker-compose up -d
   ```

### Update Theme Only

```bash
# Edit custom.css with your changes

# Rebuild the image
docker-compose build --no-cache

# Restart the container
docker-compose up -d
```

## Troubleshooting

### Theme Not Applying

1. **Clear browser cache**: Hard refresh with `Ctrl+Shift+R` (Windows/Linux) or `Cmd+Shift+R` (Mac)
2. **Check CSS file**: Ensure `custom.css` is properly copied in Dockerfile
3. **Rebuild image**: Run `docker-compose build --no-cache`
4. **Check logs**: Run `docker-compose logs open-webui-dark`

### Container Won't Start

1. **Check port conflicts**: Ensure port 3000 is not in use
   ```bash
   # Windows
   netstat -ano | findstr :3000
   
   # Linux/Mac
   lsof -i :3000
   ```
2. **Check Docker logs**: `docker-compose logs`
3. **Verify volumes**: Ensure Docker has permission to create volumes

### Logos Not Showing

1. **Verify file paths**: Ensure favicon files are in the theme directory
2. **Check file formats**: Use SVG, PNG, and ICO formats
3. **Rebuild image**: Run `docker-compose build --no-cache`
4. **Clear browser cache**: Hard refresh the page

## Development

### Testing Changes Locally

```bash
# Make changes to custom.css

# Rebuild without cache
docker-compose build --no-cache

# Restart services
docker-compose restart

# Watch logs
docker-compose logs -f
```

### Browser DevTools

Use browser DevTools to test CSS changes before applying:

1. Open DevTools (F12)
2. Go to Elements/Inspector tab
3. Modify styles in real-time
4. Copy working changes to `custom.css`
5. Rebuild image

## Project Structure

```
theme/
├── Dockerfile              # Docker image definition
├── docker-compose.yml      # Docker Compose configuration
├── custom.css             # Theme stylesheet
├── favicon.svg            # Logo (SVG format)
├── favicon.png            # Logo (PNG format)
├── favicon.ico            # Logo (ICO format)
├── .dockerignore          # Docker build exclusions
└── README.md              # This file
```

## Technical Details

- **Base Image**: `ghcr.io/open-webui/open-webui:main`
- **CSS Injection**: Custom CSS copied to `/app/build/static/custom.css`
- **Favicon Replacement**: Custom logos copied to `/app/build/static/`
- **Port**: 8080 (internal) → 3000 (external)
- **Volumes**: Persistent data storage for Open WebUI

## Color Palette

| Color | Hex | Usage |
|-------|-----|-------|
| Primary Background | `#1a1a1a` | Main background |
| Secondary Background | `#2d2d2d` | Cards, sidebar |
| Tertiary Background | `#3a3a3a` | Inputs, dropdowns |
| Accent Primary | `#6366f1` | Buttons, links |
| Accent Secondary | `#8b5cf6` | Highlights |
| Text Primary | `#f5f5f5` | Main text |
| Text Secondary | `#a3a3a3` | Secondary text |

## Accessibility

This theme follows WCAG 2.1 Level AA guidelines:
- ✅ Contrast ratio > 4.5:1 for normal text
- ✅ Contrast ratio > 3:1 for large text
- ✅ Focus indicators on interactive elements
- ✅ Reduced motion support
- ✅ High contrast mode support

## License

This theme is provided as-is for use with Open WebUI.

## Credits

- Based on [Open WebUI](https://github.com/open-webui/open-webui)
- Theme structure inspired by [Sliplane Open WebUI Theme](https://github.com/sliplane/open-webui-theme)

## Support

For issues related to:
- **Theme**: Open an issue in this repository
- **Open WebUI**: Visit [Open WebUI GitHub](https://github.com/open-webui/open-webui)
- **Docker**: Check [Docker Documentation](https://docs.docker.com/)

## Changelog

### Version 1.0.0 (Initial Release)
- Dark theme with indigo/purple accents
- Custom logo support
- Responsive design
- Accessibility enhancements
- Smooth animations and transitions
