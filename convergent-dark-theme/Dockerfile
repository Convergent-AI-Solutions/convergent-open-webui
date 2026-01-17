# Convergent Solutions Dark Mode Theme for Open WebUI
# Base Image: Open WebUI with pinned version for stability

# Subtask 1.2.1: Define base image with pinned version tag
FROM ghcr.io/open-webui/open-webui:git-49a928d

# Subtask 1.2.2: Add metadata labels (maintainer, description, version)
LABEL maintainer="convergent-solutions@example.com"
LABEL description="Convergent Solutions branded Open WebUI with custom dark mode theme"
LABEL version="1.0.0"
LABEL base-version="git-49a928d"
LABEL org.opencontainers.image.title="Convergent Solutions Open WebUI"
LABEL org.opencontainers.image.description="Custom branded Open WebUI instance for Convergent Solutions"
LABEL org.opencontainers.image.vendor="Convergent Solutions"
LABEL org.opencontainers.image.version="1.0.0"

# Subtask 1.2.3: Configure COPY commands for assets
# Copy favicon files
COPY assets/favicon.svg /app/build/static/favicon.svg
COPY assets/favicon.png /app/build/static/favicon.png
COPY assets/favicon.ico /app/build/static/favicon.ico
COPY assets/apple-touch-icon.png /app/build/static/apple-touch-icon.png

# Copy logo files
COPY assets/logo.svg /app/build/static/logo.svg
COPY assets/logo.png /app/build/static/logo.png

# Copy splash screen
COPY assets/splash-dark.png /app/build/static/splash-dark.png

# Subtask 1.2.4: Configure COPY command for custom.css
COPY custom.css /app/build/static/custom.css

# Subtask 1.2.5: Set environment variables (WEBUI_NAME, WEBUI_URL)
ENV WEBUI_NAME="CSI"
ENV WEBUI_URL="https://ai.convergentsolutions.com"

# Expose the default Open WebUI port
EXPOSE 8080

# Use the default CMD from the base image
# CMD is inherited from ghcr.io/open-webui/open-webui
