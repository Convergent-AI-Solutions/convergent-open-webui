# 🚀 Convergent Dark Theme (MVP) - Quickstart Guide

This guide will get you up and running with the **Convergent Solutions Dark Theme** MVP in minutes.

## Prerequisites
- **Docker Desktop** installed and running.

## 1. Build the Image
Open your terminal in this directory (`convergent-dark-theme`) and run:

```powershell
docker build -t convergent-dark-theme:mvp .
```

*This may take a few minutes as it downloads the base Open WebUI image.*

## 2. Run the Container
Start the container with the following command. This enables authentication and sign-ups so you can create your admin account.

```powershell
docker run -d `
  --name convergent-mvp `
  -p 8080:8080 `
  -e WEBUI_AUTH=true `
  -e ENABLE_SIGNUP=true `
  convergent-dark-theme:mvp
```

## 3. Access the Application
1.  Open your browser to [http://localhost:8080](http://localhost:8080).
2.  You should see the **Convergent Login Page** (Dark Mode).
3.  Click **"Sign Up"** to create the first account (this will be the Admin account).

## 4. Verification Check
Once logged in, verify:
-   **Dark Header**: The top navigation bar should be seamless dark slate.
-   **Blue Buttons**: The "New Chat" and "Send" buttons should be Convergent Blue (`#0066cc`).
-   **Sidebar**: The sidebar should have a dark slate background.

## Troubleshooting
-   **"Container name already in use"**: Run `docker rm -f convergent-mvp` and try step 2 again.
-   **"Port already allocated"**: Change `-p 8080:8080` to something else like `-p 8090:8080`.
