# Custom Dark Theme for Open WebUI
# Base image with specific version tag for stability
FROM ghcr.io/open-webui/open-webui:main

# Metadata
LABEL maintainer="your-email@example.com"
LABEL description="Custom Dark Theme for Open WebUI"
LABEL version="1.0.0"

# Copy custom favicon icons (add your logos here)
COPY favicon.svg /app/build/static/favicon.svg
COPY favicon.png /app/build/static/favicon.png
COPY favicon.ico /app/build/static/favicon.ico

# Copy custom CSS file
COPY custom.css /app/build/static/custom.css

# Optional: Set custom environment variables
ENV WEBUI_NAME="Open WebUI Dark"
