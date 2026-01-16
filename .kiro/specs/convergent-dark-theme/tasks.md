# Convergent Solutions Dark Mode Theme - Task List (Refined Post-MVP)

## Task Overview

This task list has been updated based on the **MVP Build Lessons**. The strategy shifts from granular, component-by-component styling to **Global System Overrides** and **Utility Class Targeting** (e.g., overriding `bg-white` globally rather than fixing every card individually).

**Total Estimated Time**: 3 Weeks (Compressed from 5)
**Strategy**: "Aggressive Global Overrides -> Targeted Fixes -> Visual Verification"

---

## Phase 1: Foundation Setup (COMPLETED)
*Status: 100% Complete*

### 1.1 Project Structure Setup
- [x] Create project directory structure (`convergent-dark-theme/`, `assets/`)
- [x] Create placeholder assets

### 1.2 Docker Configuration
- [x] Create Dockerfile with pinned base image (`git-49a928d`)
- [x] Create `docker-compose.yml`
- [x] Verify Docker build and startup

### 1.3 CSS Foundation
- [x] Create `custom.css`
- [x] Define `:root` variables (Colors, Typography, Spacing)
- [x] Implement CSS Reset/Normalization
- [x] Verify CSS loading in container

### 1.4 Placeholder Assets
- [x] Create and optimize placeholder logos/favicons/splash
- [x] Verify total asset size

---

## Phase 2: System-Wide Overrides (The "MVP + Refinement" Phase)
*Strategy: Scale the MVP approach to cover the entire application.*

### 2.1 CSS Utility Overrides (Global Dark Mode)
*Instead of styling specific components, we force-override light-mode Tailwind utilities.*
- [x] **Global Background Reset**: Force `body` and `html` to `var(--bg-dark)`
- [x] **Tailwind Light Override**: Override `.bg-white`, `.bg-gray-50`, `.bg-gray-100` to `var(--bg-dark)`
- [x] **Tailwind Text Override**: Override `.text-gray-900`, `.text-black` to `var(--text-primary)`
- [x] **Header Gradient Fix**: Override `.from-white`, `.via-white` to remove light fade effects (Fix Component: 2.1.X)

### 2.2 Critical Component Overrides
*Targeting specific shadow/complex components that resist global overrides.*
- [x] **Sidebar Implementation**: Fixed background to `var(--bg-sidebar)` (Slate 900)
- [x] **Chat Bubbles**: Implemented "Convergent Blue" gradient for user messages
- [x] **Form Inputs**: Force dark backgrounds on `input`, `textarea`, `select`
- [ ] **Modals & Dialogs**: verify background on all modal types (Settings, Delete Confirmation)
- [ ] **Dropdown Menus**: Verify background on user profile dropdowns
- [ ] **Code Blocks**: Style syntax highlighting background and copy button

### 2.3 Interactive Polish
- [ ] **Button States**: Standardize hover/active brightness filters for primary/secondary buttons
- [ ] **Focus Rings**: Ensure global matching focus ring color (`var(--cs-primary)`)
- [ ] **Scrollbars**: Apply custom dark scrollbar styling globally (Webkit)

**Estimated Time**: 3 days
**Dependencies**: MVP Build
**Acceptance Criteria**: All visible UI elements are dark; no "white flashes".

---

## Phase 3: Visual Verification & "Edge Case Hunting"
*Strategy: Use the Browser Subagent to find what we missed.*

### 3.1 Visual Regression / "Sanity" Tour
- [x] **Login Flow**: Verify Login/Signup Pages (Verified in MVP)
- [x] **Dashboard Main**: Verify Chat Interface & Sidebar (Verified in MVP)
- [ ] **Settings Menu**: Navigate to User Settings -> Verify styling of tabs and toggles
- [ ] **Mobile View**: Resize browser to <768px -> Verify Sidebar collapse and padding
- [ ] **Markdown Rendering**: Render complex markdown (tables, lists, bold) -> Verify contrast

### 3.2 Automated Asset Verification
- [ ] **Browser Subagent Crawl**:
    - [ ] Visit 5 random sub-pages (if accessible)
    - [ ] Check console for 404s on assets
    - [ ] Check for "white" computed background colors in top 50% of screen

**Estimated Time**: 2 days
**Dependencies**: Phase 2
**Acceptance Criteria**: Walkthrough artifact updated with screenshots of Settings, Mobile, and Markdown.

---

## Phase 4: Production Readiness (Testing & Assets)

### 4.1 Official Brand Integration
- [ ] Replace placeholder Assets with Official Convergent files (Logo, Favicon)
- [ ] Final Color Tuning: Update `:root` hex codes to exact brand spec
- [ ] Font Verification: Ensure no external font requests (Privacy) or bundle official fonts

### 4.2 Optimization & Bundling
- [ ] **CSS Minification**: Compress `custom.css`
- [ ] **Image Optimization**: Ensure official assets are compressed (WebP/SVG)
- [ ] **Docker Tagging**: Build final `convergent-dark-theme:1.0.0` image

### 4.3 Documentation
- [ ] Update `README.md` with:
    - [ ] Installation instructions (`docker run`)
    - [ ] Environment Variable config
    - [ ] Troubleshooting (e.g., "Clear browser cache if old theme persists")
- [ ] Create `CHANGELOG.md`

**Estimated Time**: 3 days
**Dependencies**: Phase 3
**Acceptance Criteria**: Final artifacts ready for handoff.

---

## Phase 5: Handoff & Deployment
- [ ] **Final Container Verification**: Run the 1.0.0 image on a clean machine
- [ ] **Stakeholder Demo**: Walkthrough the final UI
- [ ] **Archive**: Zip up source + assets + documentation

**Estimated Time**: 1 day
