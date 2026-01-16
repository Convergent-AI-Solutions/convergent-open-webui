# Convergent Solutions Dark Mode Theme - Task List (Refined + Maintainable)

## Task Overview

This task list incorporates **MVP Lessons** and **New User Requirements** (Loader Logo, Light Mode Support).

**Total Estimated Time**: 3-4 Weeks
**Strategy**: "Aggressive Overrides -> Refactor for Specificity -> Polish -> Verify"

---

## Phase 1: Foundation Setup (COMPLETED)
*Status: 100% Complete*

### 1.1 Project Structure & Docker (Completed)
- [x] Project directory, Assets folder, Dockerfile, docker-compose.yml created.
- [x] MVP Container verified.

### 1.2 CSS & Asset Baseline (Completed)
- [x] `custom.css` created with `:root` and aggressive overrides.
- [x] Placeholder assets created and optimized.

---

## Phase 2: System-Wide Overrides & Refactoring
*Strategy: Complete the dark mode overrides, then clean up the "nuclear" !important usage.*

### 2.1 CSS Utility Overrides (Completed in MVP)
- [x] **Global Background**: Forced dark body/html.
- [x] **Tailwind Reset**: Overridden `.bg-white`, `.text-gray-900`, etc.
- [x] **Header Fix**: Resolved white gradient issue.

### 2.2 Critical Component Overrides (In Progress)
- [x] **Sidebar**: Dark Slate background.
- [x] **Chat Bubbles**: Brand-colored user messages.
- [x] **Form Inputs**: Force dark backgrounds on `input`, `textarea`, `select`
- [x] **Modals & Dialogs**: Verified background on Settings modal (Dark #1a1a1a).
- [x] **Dropdown Menus**: Verified background on Profile/Model dropdowns.
- [x] **Code Blocks**: Verified syntax highlighting and dark background.
- [ ] **Markdown Content**: Verify lists, tables, and headers in chat responses.

### 2.3 Interactive Polish
- [ ] **Loader Branding**: Ensure page loading screen displays Convergent Logo instead of default Open WebUI logo.
- [ ] **Button States**: Standardize hover/active brightness filters for primary/secondary buttons.
- [ ] **Focus Rings**: Ensure global matching focus ring color (`var(--cs-primary)`).
- [ ] **Scrollbars**: Apply custom dark scrollbar styling globally (Webkit).

### 2.4 Theme System Evolution (Light Mode Support) [NEW]
*Goal: Enable Light/Dark toggle while maintaining Convergent Branding in both.*
- [ ] **Restore Theme Toggle**: Un-hide or restore the functionality of the UI Theme Switcher.
- [ ] **Scoped Overrides**: Refactor global "nuclear" overrides to apply only within `.dark` selector (e.g., `html.dark body`).
- [ ] **Branded Light Mode**: 
    - Create a Light Mode variable set (White backgrounds, Dark text).
    - Ensure `--cs-primary` (Blue) and Logos remain consistent in Light Mode.
- [ ] **Cleanup !important**: Remove explicit `!important` tags that prevent Light Mode from activating.

---

## Phase 3: Visual Verification & "Edge Case Hunting"
*Strategy: Use the Browser Subagent to simulate real user journeys.*

### 3.1 Visual Regression Tour
- [x] **Login Flow**: Sign Up, Sign In (Verified).
- [x] **Dashboard**: Sidebar, Chat Input (Verified).
- [x] **Interactive Components**: Modals, Dropdowns, Code Blocks (Verified).
- [ ] **Light Mode Check**: Verify toggling between Light/Dark works (after Phase 2.4).
- [ ] **Mobile Responsive**: Verify Sidebar collapse, Chat Input spacing on <768px.
- [ ] **Error States**: Trigger an error (e.g., failed connection) to check Toast/Alert styling.

### 3.2 Automated Asset & Contrast Check
- [ ] **Browser Subagent Crawl**:
    - [ ] Check for broken images (404s).
    - [ ] Calculate contrast ratio on primary buttons (Target 4.5:1).
    - [ ] Check for invisible text (foreground == background).

---

## Phase 4: Production Readiness (Testing & Assets)

### 4.1 Asset Integration & Fallback
- [ ] **Official Assets**: Replace placeholders with final designs.
- [ ] **Asset Generation (Fallback)**: If design team delays, use `generate_image` to create "Better-than-placeholder" assets for v1.0.

### 4.2 Optimization
- [ ] **CSS Minification**: Compress for production.
- [ ] **Docker Tagging**: Build `convergent-dark-theme:1.0.0` (Production).

### 4.3 Documentation
- [ ] **QUICKSTART.md**: (Created MVP version, update for Prod).
- [ ] **Admin Guide**: How to deploy, env vars, rollback.

---

## Phase 5: Handoff
- [ ] **Final Verification**: Clean install test.
- [ ] **Stakeholder Demo**.
- [ ] **Archive**.
