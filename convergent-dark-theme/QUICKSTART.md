# 🚀 Convergent Dark Theme (MVP) - Quickstart Guide

This guide will get you up and running with the **Convergent Solutions Intelligence (CSI)** branded theme in minutes.

## Prerequisites
- **Docker Desktop** installed and running.

## 1. Fast Track (Recommended)
The easiest way to run the application is using `docker-compose`. This ensures all volume mounts for the theme and assets are correctly configured.

From this directory (`convergent-dark-theme`), run:

```powershell
docker-compose up --build -d
```

*This will build the image and start the container in the background.*

## 2. Access the Application
1.  Open your browser to [http://localhost:3001](http://localhost:3001).
2.  You should see the **CSI Login Page** (Dark Mode).
3.  Click **"Sign Up"** or **"Get Started"** to create the first account (this will be the Admin account).

## 3. Verification Check
Once logged in, verify:
-   **Dark Header**: The top navigation bar should be seamless dark slate.
-   **Branding**: The application name should be **CSI**.
-   **Blue Buttons**: Primary buttons should use Convergent Blue (`#0066cc`).

## Troubleshooting
-   **"Theme is White"**: 
    1. Click on your profile icon (bottom left).
    2. Go to **Settings** > **General**.
    3. Ensure **Theme** is set to **Dark** or **OLED Dark**.
    - *Note: Our `custom.css` is mapped via volume, so changes to styling apply immediately on page refresh.*
-   **Container Issues**: Use `docker-compose down` followed by `docker-compose up --build -d` to fresh start.
-   **Port Conflict**: If port 3001 is taken, edit the `ports` section in `docker-compose.yml`.

---
*Convergent Solutions Intelligence - Built on Open WebUI*
