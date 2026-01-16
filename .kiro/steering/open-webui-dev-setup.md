# Open WebUI Local Development Setup

Essential reference guide for setting up and running Open WebUI in development mode.

## Quick Start Commands

### Initial Setup
```bash
# Clone and navigate
git clone https://github.com/open-webui/open-webui.git
cd open-webui

# Frontend setup
cp -RPp .env.example .env
npm install
npm run dev  # Runs on http://localhost:5173

# Backend setup (new terminal)
cd backend
conda create --name open-webui python=3.11
conda activate open-webui
pip install -r requirements.txt -U
sh dev.sh  # Runs on http://localhost:8080
```

## Prerequisites

- **Python**: 3.11+
- **Node.js**: 22.10+
- **OS**: Linux/WSL, Windows 11, or macOS
- **IDE**: VS Code recommended

## Critical Rules

### Data Separation
**NEVER share database or data directory between dev and production.**
- Dev migrations may not be backward-compatible
- Keep dev data completely separate

### Stay Updated
```bash
git pull origin dev  # Run frequently
```

### Terminal Management
- Use **separate terminals** for frontend and backend
- VS Code integrated terminals recommended
- Frontend: project root (`open-webui/`)
- Backend: backend directory (`open-webui/backend/`)

## Environment Setup

### Frontend (.env file)
```bash
# Copy example config
cp -RPp .env.example .env

# Default settings usually sufficient for local dev
# Customize if needed, but don't commit secrets
```

### Backend (Python Environment)

**Option 1: Conda (Recommended)**
```bash
conda create --name open-webui python=3.11
conda activate open-webui
```

**Option 2: venv**
```bash
python3 -m venv venv
source venv/bin/activate  # Linux/macOS
.\venv\Scripts\activate   # Windows
```

## Development Workflow

### Starting Development
1. **Terminal 1 (Frontend)**:
   ```bash
   cd open-webui
   npm run dev
   ```

2. **Terminal 2 (Backend)**:
   ```bash
   cd open-webui/backend
   conda activate open-webui  # if using conda
   sh dev.sh
   ```

3. **Access**:
   - Frontend: http://localhost:5173
   - Backend API Docs: http://localhost:8080/docs

### Building Frontend
```bash
npm run build  # Test production build
```

## Testing from Other Devices

### Frontend Only
1. Find LAN IP: `192.168.1.42` (example)
2. Access from device: `http://192.168.1.42:5173`

### Full Stack (Frontend + Backend)
1. Edit `backend/dev.sh`:
   ```bash
   export CORS_ALLOW_ORIGIN="http://localhost:5173;http://localhost:8080;http://192.168.1.42:5173"
   ```
2. Restart backend: `sh dev.sh`
3. Access from device: `http://192.168.1.42:5173`

**Security**: Don't use wildcard `*` in production

## Common Issues & Solutions

### Memory Error: "FATAL ERROR: Reached Heap Limit"
```bash
# Increase Node.js memory
export NODE_OPTIONS="--max-old-space-size=4096"  # Linux/macOS
set NODE_OPTIONS=--max-old-space-size=4096       # Windows CMD
$env:NODE_OPTIONS="--max-old-space-size=4096"    # Windows PowerShell

npm run dev
```

### Port Conflicts

**Find process using port**:
```bash
# Linux/macOS
lsof -i :5173
lsof -i :8080

# Windows
netstat -ano | findstr :5173
netstat -ano | findstr :8080
```

**Kill process**:
```bash
# Linux/macOS
kill <PID>

# Windows (as admin)
taskkill /PID <PID> /F
```

### Hot Reload Not Working

1. Verify both servers running
2. Hard refresh browser: `Ctrl+Shift+R` (Win/Linux) or `Cmd+Shift+R` (Mac)
3. Clear browser cache or use incognito
4. Reinstall frontend dependencies:
   ```bash
   rm -rf node_modules && npm install
   # or if needed: npm install --force
   ```
5. Restart backend for backend changes

### Icons Not Loading (CORS)

Configure `CORS_ALLOW_ORIGIN` in backend:
```bash
# In backend/dev.sh or .env
export CORS_ALLOW_ORIGIN="http://localhost:5173"
```

### Dependency Installation Issues

**Frontend**:
```bash
npm install --force  # If compatibility warnings
```

**Backend**:
```bash
pip install -r requirements.txt -U  # Ensure latest versions
```

## Contributing Workflow

### Branch Management
```bash
# Never commit to dev directly
git checkout dev
git pull origin dev
git checkout -b my-feature-branch

# Work on your feature...

# Stay synced with dev
git checkout dev
git pull origin dev
git checkout my-feature-branch
git merge dev
```

### Commit Guidelines
- Small, logical commits
- Clear commit messages
- Example: `Fix: Corrected typo in backend setup docs`
- Example: `Feat: Added user profile page`

### Pull Request Process
1. Test your changes
2. Run available tests
3. Submit PR to `dev` branch
4. Provide clear title and description
5. Link related issues
6. Respond to feedback

## Project Structure

```
open-webui/
├── backend/           # Python backend (FastAPI)
│   ├── dev.sh        # Backend dev server script
│   ├── requirements.txt
│   └── open_webui/   # Main backend code
├── src/              # Frontend source (Svelte)
├── static/           # Static assets
├── .env              # Frontend environment config
├── package.json      # Frontend dependencies
└── vite.config.js    # Frontend build config
```

## Key URLs

- **Frontend Dev**: http://localhost:5173
- **Backend API**: http://localhost:8080
- **API Docs**: http://localhost:8080/docs
- **Repository**: https://github.com/open-webui/open-webui

## Best Practices

1. **Start Small**: Begin with docs, bug fixes, or minor UI improvements
2. **Discuss First**: Propose major features via GitHub issues
3. **Test Regularly**: Run dev setup frequently to catch bugs early
4. **Keep Updated**: Pull from dev branch regularly
5. **Separate Data**: Never mix dev and production data
6. **Use Branches**: One branch per feature/fix
7. **Document Changes**: Clear commit messages and PR descriptions

## Quick Troubleshooting Checklist

- [ ] Both terminals running (frontend + backend)?
- [ ] Correct directories (root for frontend, backend/ for backend)?
- [ ] Python environment activated (conda/venv)?
- [ ] Dependencies installed (npm install, pip install)?
- [ ] Ports available (5173, 8080)?
- [ ] .env file configured?
- [ ] Latest code pulled (git pull origin dev)?
- [ ] Browser cache cleared?

## Getting Help

- Check API docs: http://localhost:8080/docs
- Review full guide: `docs/LocalDevelopmentGuide.md`
- Open GitHub issue for bugs
- Join community channels (check README)

---

*This is a quick reference. For comprehensive details, see `docs/LocalDevelopmentGuide.md`*
