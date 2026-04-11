---
phase: "01"
plan: "04"
subsystem: spa-bootstrap
tags: [spa, routing, view-system, url-utils]
dependency_graph:
  requires: [01-01, 01-02, 01-03]
  provides: [spa-bootstrap, view-routing, url-query-params]
  affects: [assets/app.js]
tech_stack:
  added: []
  patterns: [spa-view-toggle, url-sanitization, async-bootstrap]
key_files:
  created: []
  modified:
    - assets/app.js
decisions:
  - "showView() iterates all 4 view IDs on every call using classList.toggle with boolean force argument"
  - "sanitizeId() step order: trim/lowercase → + → @ → . → strip non-alphanumeric"
  - "init() calls showView('loading-view') synchronously as first statement before any await"
  - "Invalid config treated same as network failure — both caught by catch block"
  - "id/certificate branch left as stub; Phase 02 will wire fetchAttendee"
  - "search branch left as stub; Phase 04 will wire renderSearchView"
metrics:
  duration: "~5 minutes"
  completed: "2026-04-11"
---

# Phase 01 Plan 04: SPA Shell Summary

## One-liner

SPA bootstrap wired end-to-end: loading → config fetch → CSS vars → correct view (or error), with showView(), getQueryParam(), and sanitizeId() utilities.

## Tasks Completed

| # | Task | Commit | Status |
|---|------|--------|--------|
| 1 | Add showView(), getQueryParam(), sanitizeId() to app.js | 748c91b | ✅ |
| 2 | Replace stub init() with full bootstrap implementation | de9552f | ✅ |

## Final app.js Structure

1. `'use strict';`
2. `document.addEventListener('DOMContentLoaded', init);`
3. `async function init()` — full bootstrap: loading → fetch → validate → applyVars → title → view routing
4. `// === Config Loader ===`
5. `async function fetchConfig()` — fetch + HTTP error throw
6. `function validateConfig(config)` — required fields check
7. `// === CSS Variable Injection ===`
8. `function applyConfigVars(config)` — sets all :root CSS vars
9. `// === SPA View System ===`
10. `function showView(activeId)` — toggles hidden on all 4 view divs
11. `// === URL Utilities ===`
12. `function getQueryParam(name)` — URLSearchParams wrapper
13. `function sanitizeId(email)` — email → safe ID string

## Deviations from Plan

None — plan executed exactly as written.

## Self-Check: PASSED

- assets/app.js exists and contains all declared functions ✅
- Commits 748c91b and de9552f present in git log ✅
- init() starts with showView('loading-view') before any await ✅
- catch block logs with [App] prefix and calls showView('error-view') ✅
- No .innerHTML usage in new code ✅
