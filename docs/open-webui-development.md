---
inclusion: always
---

# Open WebUI Development Guide

This steering file provides essential context and guidelines for developing features in the Open WebUI project.

## Project Overview

Open WebUI is an extensible, feature-rich, and user-friendly self-hosted AI platform designed to operate entirely offline. It supports various LLM runners like Ollama and OpenAI-compatible APIs, with a built-in inference engine for RAG.

## Technology Stack

### Frontend
- **Framework**: SvelteKit 2.x with Svelte 5.x
- **Language**: TypeScript 5.x
- **Styling**: Tailwind CSS 4.x
- **Build Tool**: Vite 5.x
- **Testing**: Vitest, Cypress for E2E
- **Key Libraries**:
  - TipTap for rich text editing
  - Chart.js for visualizations
  - CodeMirror for code editing
  - Socket.io for real-time communication
  - Pyodide for Python runtime in browser

### Backend
- **Framework**: FastAPI 0.128.0
- **Language**: Python 3.11-3.12
- **Server**: Uvicorn with standard extras
- **Database**: 
  - SQLAlchemy 2.0.45 (ORM)
  - Peewee 3.18.3 (alternative ORM)
  - Alembic 1.17.2 (migrations)
  - Support for SQLite, PostgreSQL
- **Vector Databases**: ChromaDB, PGVector, Qdrant, Milvus, Elasticsearch, OpenSearch, Pinecone, S3Vector, Oracle 23ai
- **Authentication**: 
  - Python-JOSE for JWT
  - Authlib for OAuth
  - LDAP3 for directory integration
- **AI/ML**:
  - LangChain for LLM orchestration
  - Transformers & Sentence-Transformers
  - OpenAI, Anthropic, Google GenAI clients
  - MCP (Model Context Protocol) 1.25.0
- **Task Scheduling**: APScheduler
- **Caching**: Redis with aiocache
- **Observability**: OpenTelemetry support

## Project Structure

```
convergent-open-webui/
├── backend/              # Python FastAPI backend
├── src/                  # Svelte frontend source
├── static/               # Static assets
├── docs/                 # Documentation
├── cypress/              # E2E tests
├── test/                 # Test files
├── scripts/              # Build and utility scripts
├── docker-compose.yaml   # Docker orchestration
├── Dockerfile            # Container definition
├── package.json          # Node dependencies
└── pyproject.toml        # Python dependencies
```

## Development Setup

### Prerequisites
- **Operating System**: Linux (or WSL on Windows), Windows 11, or macOS
- **Node.js**: 22.10 or higher (minimum 18.13.0, max 22.x.x)
- **Python**: 3.11 or higher (3.11-3.12 supported, NOT 3.13+)
- **npm**: 6.0.0+
- **IDE**: VS Code recommended for integrated terminal management
- **Docker** (optional, for containerized development)
- **Conda** (recommended) or Python venv for environment isolation

### Critical Development Rules

⚠️ **NEVER share data between dev and production**
- Dev builds may include database migrations that are NOT backward-compatible
- Keep development database and data directory completely separate from production
- Rolling back after a dev migration can break production

⚠️ **Stay updated with dev branch**
- Run `git pull origin dev` frequently to stay current
- Test regularly - community testing is essential for quality releases

### Initial Setup

#### 1. Clone Repository
```bash
git clone https://github.com/open-webui/open-webui.git
cd open-webui
```

#### 2. Frontend Setup

**Configure Environment:**
```bash
# Copy environment template
cp -RPp .env.example .env

# Edit .env as needed (defaults usually sufficient for local dev)
# DO NOT commit sensitive information to .env
```

**Install Dependencies:**
```bash
npm install

# If you encounter compatibility warnings/errors:
npm install --force
```

**Start Development Server:**
```bash
npm run dev
```

Access at: http://localhost:5173

**Build Frontend (Recommended):**
```bash
# Verify production build works
npm run build
```

#### 3. Backend Setup

**Use separate terminal for backend** (VS Code: Terminal > New Terminal)

**Navigate to backend:**
```bash
cd backend
```

**Create Python Environment (Conda - Recommended):**
```bash
conda create --name open-webui python=3.11
conda activate open-webui
```

**Alternative: Python venv:**
```bash
# Create venv
python3 -m venv venv

# Activate (Linux/macOS)
source venv/bin/activate

# Activate (Windows)
.\venv\Scripts\activate
```

**Install Dependencies:**
```bash
pip install -r requirements.txt -U
```

**Start Backend Server:**
```bash
sh dev.sh
```

API Documentation: http://localhost:8080/docs

### Local Development Commands

#### Frontend Development
```bash
# Start dev server with hot reload (port 5173)
npm run dev

# Start on custom port 5050
npm run dev:5050

# Build for production
npm run build

# Build with watch mode
npm run build:watch

# Preview production build
npm preview
```

#### Code Quality
```bash
# Run all linters (frontend + types + backend)
npm run lint

# Lint frontend only
npm run lint:frontend

# Type checking
npm run lint:types

# Lint Python backend
npm run lint:backend

# Format frontend code
npm run format

# Format Python code
npm run format:backend
```

#### Testing
```bash
# Run frontend tests
npm test:frontend

# Open Cypress for E2E testing
npm run cy:open
```

#### Internationalization
```bash
# Parse and update translation files
npm run i18n:parse
```

### Testing From Another Device

To access your dev instance from phone/tablet on same Wi-Fi:

1. **Find your dev machine's LAN IP** (e.g., 192.168.1.42)

2. **Frontend only (quick check):**
   - Keep backend on localhost
   - Browse to: `http://192.168.1.42:5173`

3. **Full stack (frontend + backend):**
   - Edit `backend/dev.sh` and add your LAN address to CORS:
   ```bash
   export CORS_ALLOW_ORIGIN="http://localhost:5173;http://localhost:8080;http://192.168.1.42:5173"
   ```
   - Restart backend: `sh dev.sh`
   - Browse to: `http://192.168.1.42:5173`

⚠️ **Security Note**: Wildcard `"*"` works but NEVER use in production

### Docker Development

```bash
# Start all services (Ollama + Open WebUI)
docker-compose up

# Start with GPU support
docker-compose -f docker-compose.gpu.yaml up

# Start API-only mode
docker-compose -f docker-compose.api.yaml up
```

## Development Guidelines

### Code Style

#### Frontend (TypeScript/Svelte)
- Use TypeScript for type safety
- Follow ESLint configuration (`.eslintrc.cjs`)
- Use Prettier for formatting (`.prettierrc`)
- Component files use `.svelte` extension
- Prefer composition over inheritance
- Use Svelte 5 runes (`$state`, `$derived`, `$effect`)

#### Backend (Python)
- Follow PEP 8 style guide
- Use Black for formatting (line length: default)
- Use Pylint for linting
- Type hints are encouraged
- Async/await for I/O operations
- Use Pydantic for data validation

### Git Workflow

1. **Never commit directly to dev branch** - Always use feature branches

2. **Branch Naming**: Use descriptive names
   - `feature/feature-name`
   - `fix/bug-description`
   - `docs/documentation-update`

3. **Creating a Feature Branch**:
   ```bash
   git checkout dev
   git pull origin dev  # Ensure local dev is up-to-date
   git checkout -b my-feature-branch
   ```

4. **Stay Synced with dev Branch**:
   ```bash
   git checkout dev
   git pull origin dev
   git checkout my-feature-branch
   git merge dev
   # Resolve any merge conflicts
   ```

5. **Commit Messages**: Clear and descriptive
   - Use present tense ("Add feature" not "Added feature")
   - Examples:
     - `Fix: Corrected typo in documentation for backend setup`
     - `Feat: Implemented user profile page with basic information display`
   - Reference issues when applicable

6. **Pull Requests**:
   - Open discussion before major changes
   - Complete PR checklist
   - Include tests for new features
   - Update documentation
   - Keep PRs focused and timely
   - Submit to `dev` branch (not `main`)
   - Be responsive to feedback and prepared to make revisions

### Testing Requirements

#### Frontend Tests
- Use Vitest for unit tests
- Use Cypress for E2E tests
- Test files: `*.test.ts` or `*.spec.ts`
- Aim for meaningful test coverage
- Run tests before submitting PRs: `npm test:frontend`

#### Backend Tests
- Use pytest for Python tests
- Test API endpoints
- Test database operations
- Mock external services

#### Pre-Push Checklist
- Run available tests (check `package.json` for test commands)
- Verify no linting errors: `npm run lint`
- Ensure build succeeds: `npm run build`
- Test in browser manually
- Check API docs still work: http://localhost:8080/docs

### Documentation

- Update relevant documentation in `docs/` directory
- Add JSDoc comments for complex functions
- Include docstrings for Python functions/classes
- Update README if adding new features

### Internationalization (i18n)

- Translation files: `src/lib/i18n/locales/{language-code}/`
- Use JSON format for translations
- Add language codes to `src/lib/i18n/locales/languages.json`
- Submit translations in standalone PRs
- Preserve JSON structure when translating

## Common Development Tasks

### Starting Fresh Development Session

1. **Update local dev branch**:
   ```bash
   git checkout dev
   git pull origin dev
   ```

2. **Activate Python environment**:
   ```bash
   conda activate open-webui
   # or: source venv/bin/activate
   ```

3. **Start backend** (in backend directory):
   ```bash
   cd backend
   sh dev.sh
   ```

4. **Start frontend** (in separate terminal, project root):
   ```bash
   npm run dev
   ```

### Adding a New Feature

1. **Discuss first**: Open GitHub discussion for major changes
2. **Create feature branch**:
   ```bash
   git checkout dev
   git pull origin dev
   git checkout -b feature/my-feature
   ```
3. **Implement with tests**
4. **Update documentation**
5. **Run linters and tests**:
   ```bash
   npm run lint
   npm test:frontend
   npm run build
   ```
6. **Submit PR** with completed checklist

### Fixing a Bug

1. **Verify bug exists** in latest dev branch
2. **Create fix branch**:
   ```bash
   git checkout -b fix/bug-description
   ```
3. **Write test that reproduces bug** (if applicable)
4. **Implement fix**
5. **Verify test passes**
6. **Submit PR**

### Starting Small (Recommended for New Contributors)

- **Documentation improvements**: Fix typos, clarify explanations
- **Bug fixes**: Address reported issues
- **Small UI enhancements**: Styling, minor layout fixes
- These help you learn the codebase before tackling larger features

### Adding Dependencies

#### Frontend
```bash
npm install <package-name>
```

#### Backend
Add to `pyproject.toml` under `dependencies` array:
```toml
dependencies = [
    "package-name==version",
]
```

## Environment Variables

Key environment variables for development:

### Backend
- `OLLAMA_BASE_URL`: Ollama server URL (default: http://localhost:11434)
- `WEBUI_SECRET_KEY`: Secret key for sessions
- `DATABASE_URL`: Database connection string
- `OPENAI_API_KEY`: OpenAI API key (if using OpenAI)
- `HF_HUB_OFFLINE`: Set to `1` for offline mode

### Frontend
- `VITE_*`: Vite-specific environment variables

## Troubleshooting

### Common Issues

#### 1. "FATAL ERROR: Reached Heap Limit" (Frontend)

Node.js running out of memory during build process.

**Solution - Increase Node.js heap size:**

```bash
# Linux/macOS (bash, zsh)
export NODE_OPTIONS="--max-old-space-size=4096"
npm run dev

# Windows (Command Prompt)
set NODE_OPTIONS=--max-old-space-size=4096
npm run dev

# Windows (PowerShell)
$env:NODE_OPTIONS="--max-old-space-size=4096"
npm run dev
```

- `4096` = 4GB memory (try `8192` for 8GB if needed)
- Setting applies only to current terminal session
- Ensure system has sufficient RAM (4GB+ recommended)

**For Docker:** Add to Dockerfile:
```dockerfile
ENV NODE_OPTIONS=--max-old-space-size=4096
```

#### 2. Port Conflicts (Address Already in Use)

Another application is using port 5173 (frontend) or 8080 (backend).

**Identify conflicting process:**

```bash
# Linux/macOS
lsof -i :5173
netstat -tulnp | grep 5173

# Windows (Command Prompt)
netstat -ano | findstr :5173

# Windows (PowerShell)
Get-Process -Id (Get-NetTCPConnection -LocalPort 5173).OwningProcess
```

**Terminate process:**

```bash
# Linux/macOS
kill <PID>
# Force kill if needed (use with caution)
kill -9 <PID>

# Windows (as administrator)
taskkill /PID <PID> /F
```

**Alternative - Change ports:**
- Frontend: Modify `vite.config.js` or `.env`
- Backend: Modify `dev.sh` or backend config
- Update frontend `.env` to point to new backend URL

#### 3. Hot Reload Not Working

**Troubleshooting steps:**

1. **Verify servers running**: Check both `npm run dev` and `sh dev.sh` are active
2. **Check for HMR messages**: Look for "HMR enabled" or "watching for file changes"
3. **Hard refresh browser**:
   - Windows/Linux: `Ctrl+Shift+R`
   - macOS: `Cmd+Shift+R`
   - Or use private/incognito window
4. **Refresh dependencies**:
   ```bash
   rm -rf node_modules && npm install
   # If fails, try: npm install --force
   ```
5. **Backend changes**: Manually restart backend server for significant backend changes
6. **Restart IDE**: In rare cases, restart your code editor

#### 4. Icons Not Loading (CORS Issues)

Frontend cannot request resources from backend due to CORS restrictions.

**Solution**: Configure `CORS_ALLOW_ORIGIN` in backend:

- Edit `backend/dev.sh` or `.env`
- Add frontend URL: `http://localhost:5173`
- Example:
  ```bash
  export CORS_ALLOW_ORIGIN="http://localhost:5173;http://localhost:8080"
  ```
- Restart backend server

#### 5. Dependency Issues

**Frontend:**
```bash
# Clear and reinstall
rm -rf node_modules package-lock.json
npm install

# If compatibility errors
npm install --force
```

**Backend:**
```bash
# Reinstall with latest versions
pip install -r requirements.txt -U --force-reinstall
```

### Common Development Issues

1. **Docker Connection Issues**
   - Use `--network=host` flag
   - Check `OLLAMA_BASE_URL` configuration
   - Verify port mappings

2. **Build Failures**
   - Clear `node_modules` and reinstall
   - Check Node.js version compatibility (22.10+ recommended)
   - Verify Python version (3.11-3.12 only)

3. **Database Issues**
   - Check database migrations with Alembic
   - Verify database permissions
   - Check connection strings
   - **Never share dev and production databases**

## Resources

- **Documentation**: https://docs.openwebui.com/
- **Local Development Guide**: https://docs.openwebui.com/getting-started/advanced-topics/development
- **Discord Community**: https://discord.gg/5rJgQTnV4s
- **GitHub Issues**: https://github.com/open-webui/open-webui/issues
- **GitHub Discussions**: https://github.com/open-webui/open-webui/discussions
- **Docs Repository**: https://github.com/open-webui/docs
- **API Documentation** (when backend running): http://localhost:8080/docs

## Testing Docker Dev Image

If local setup is too complex, test using dev Docker image:
```bash
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui --add-host=host.docker.internal:host-gateway --restart always ghcr.io/open-webui/open-webui:dev
```

⚠️ **Warning**: `:dev` branch contains latest unstable features - use at your own risk

## Important Notes

### Scope Boundaries

- **Open WebUI**: Web interface and user experience
- **Ollama**: Core LLM technology (separate project)
- Direct Ollama issues to: https://ollama.com/

### Docker Knowledge

- Basic Docker understanding is assumed
- Refer to Docker documentation for fundamentals
- Advanced configurations (reverse proxies, HTTPS) require additional knowledge

### Security

- Never report vulnerabilities via GitHub Issues
- Use GitHub Security reporting functionality
- Follow security guidelines in `docs/SECURITY.md`

## License

This project uses multiple licenses. See `LICENSE` and `LICENSE_HISTORY` for details. The "Open WebUI" branding must be preserved per license requirements.
