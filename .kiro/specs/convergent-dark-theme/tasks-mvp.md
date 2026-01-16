# Convergent MVP Theme - Rapid Implementation Tasks

## Goal
Deliver a functional, branded "Convergent Dark" theme in **under 2 days** by focusing on high-impact CSS overrides and essential assets only.

## 1. Core Branding (The "Face")
- [x] **Global Color Swap**
  - [x] Set `:root` generic variables in `custom.css` (Backgrounds, Primary, Text)
  - [x] Force-override generic Tailwind classes if necessary (e.g., `.bg-white` -> `var(--bg-primary)`)
- [x] **Logo & Assets**
  - [x] Use existing placeholder assets (Task 1.4 completed)
  - [x] Ensure logo is visible on Login & Sidebar

## 2. High-Impact Component Styling
- [x] **Chat Interface (The Core)**
  - [x] Style User Message Bubble (Blue/Teal Gradient or Accent)
  - [x] Style AI Message Bubble (Dark Grey)
- [x] **Sidebar Navigation**
  - [x] Dark background
  - [x] Selected item highlight (Brand Color)
- [x] **Login Page**
  - [x] Quick CSS fix for the white overlay (transparency/dark mode)

## 3. Deployment
- [x] **Docker Build**
  - [x] Re-use existing `Dockerfile`
  - [x] Verify build
- [x] **Manual Sanity Check**
  - [x] Login -> Chat -> Settings (Verify readable text)

---

## Estimated Timeline
- **Setup & Colors**: 2 Hours
- **Components**: 3 Hours
- **Review & Fixes**: 1 Hour
**Total**: ~6 Hours (1 Day)
